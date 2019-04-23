# Magento目录结构
##### 目录结构如下：
![Structure](/image/structure.png)
  > 此目录结构截取自github上

####  1. 主题目录
  > 通常在谈论自定义主题或者其他任何主题时使用,对于Magento开箱即用的前端主题，不同的下载平台，代码也就存在在不同的目录里。
* app/dsign/fronted/Magento/`theme`
  > 通过github下载，代码在`app/dsign/fronted/Magento/(theme)`
* vendor/magento/theme-fronted-`theme`
  > 通过composer安装或者官网下载的源码包，代码在`vendor/magento/theme-fronted-(theme)`


#### 2. 模块目录
* app/code/Magento/`Module`
  > 通过github下载，核心代码就在`app/code/Magento/(Module)`
* vendor/magento/module-`module`-`name`
  > 通过composer安装或者官网下载的源码包，核心代码在`vendor/magento/module-(module)-(name)`

#### 3. 核心代码目录结构细讲（以上图为例）
##### 3.1 app
  app目录存放网站源代码。
  所有的`插件/主题/js/css`等等都放在这个目录
##### 3.2 bin
  M2提供的命令行工具。
  例如：
  ``` 
    php bin/magento setup:upgrate
    php bin/magento setup:static-content:deploy -f
  ```
##### 3.3 dev
  M2的单元测试代码。
  普通用户一般用不到，M2的官方开发者会用到
##### 3.4 generated（特殊）
  在M2里可以定义一些虚拟类
  这些类是自动生成的，会放在generated目录里，相当于php代码缓存。
  > 如果有修改php的构造函数`function _construct()`里的代码，就要用`rm generated/* -rf`,不然会出现找不到类或者其他奇怪的错误。
##### 3.5 lib
  存放M2自带的公用`js/jquery插件和字体`。
  一般用不到。
##### 3.6 phpserver
  存放php内置的web服务器。
  用来替代浏览器，直接在命令行打开网站。
  一般用不到。
##### 3.7 pub
  存放图片文件（比如产品图片）以及生成静态缓存文件。
  经常会用到。
##### 3.8 setup
  安装目录。
##### 3.9 var
  存放`cache/log/report/export/page cache缓存文件`。
  进场会用到。
##### 3.10  vendor
  存放第三方php组件。
  也就是composer install后下载安装的第三方php组件。
  如果是composer安装或者官网下载的源码包，那么M2核心源代码就在`vendor/magento`里，这样我们在开发过程中，会经常调试`vendor/magento`里的核心代码。
##### 3.11  .htaccess
  apache服务器配置文件。
  如果使用的是apache服务的话，就会用到这个文件。
##### 3.12  composer.json
  M2依赖的各种库文件。
  > 和vue脚手架搭建出来的`package.json`作用一样，项目的依赖。
##### 3.13  index.php
  入口文件。
  > 注意：M2的默认nginx配置的入口文件是在`pub/index.php`里，考虑安全性，不暴露`app/`文件夹。
##### 3.14  nginx.conf.sample
  M2官方推荐的配置文件。
  如果使用的是nginx服务的话，就会用到这个文件。

####  4. app结构重点讲解
#####  app目录机构如下：
  ![app structure](/image/app_structure.png)
  > 此目录截取与github上
##### 4.1 code
  存放插件和核心代码。
  安装后，就会发现已经存在的`code/Magento`目录。
  > 注意：以后自己写的插件或者第三方插件都放在`code`下面。
##### 4.2 design
  存放前后台主题。
##### 4.3 design目录结构如下：
  ![design_structure](/image/design_structure.png)
  > 此目录截取与github上

  `adminhtml/Magento/bckend`存放后台主题
  `fronted/Magento`存放前台主题
  > 注意：一般不需要修改后台主题，主要是修改前台主题。但是不要直接修改`frontend/Magento默认主题`，通常我们都是自己新建一个主题，在自己的主题里修改。

##### 4.4 etc
  M2系统配置文件。
  ***不要动！！！***

##### 4.5 i18n
  M2的语言包存放目录。
  > 存放所有的语言包，包括安装的三方语言包。


#### 5. pub
##### pub目录结构如下：
  ![pub structure](/image/pub_structure/png)
  > 此目录截取于github上
  
  > 这里有一个index.php文件，和项目根目录下的index.php是一样的。自带的nginx.conf.sample是把pub/index.php设置成入口路径。当然也可以把入口路径改为`项目根目录/index.php`也没有问题，但是不建议这样做，因为暴露了`app/`目录，不太安全。一般是读取`pub/`文件夹下deploy生成的文件，保证了`app/`目录的安全性。
  
##### 5.1 errors
  存放404/503错误页面
  ***不要动***
##### 5.2 media
  存放`分类图片/产品图片/下载文件/其他后台上传的图片或者文件`。
  ***不要动***
##### 5.3 static
  ***非常重要***
  deploy命令执行后，就会在此生成网站静态文件，也包括前后台的静态文件。
  + `phtml(模板文件)/js/css`
    > - 也就是把所有插件（code）里的代码都生成静态文件
    > - 开启缓存的情况下，前台读取的都是这里`js/css/模板`
    > - 开启缓存的情况下, 不能直接改`code/design`里的源代码，改了不会生效，直接改static下边的文件就可以（reason：它生成的目录结构跟插件的目录结构是一样的）。

####  6. 重要掌握
+ 分类和产品页面 -> Catelog
+ customer用户中心页面  -> Customer
+ 购物车页面  -> Checkout
+ 支付页面  ->  Checkout
+ 订单页面  ->  Sales
+ 搜索页面  ->  Search
+ 首页及cms页面 ->  Cms
+ 联系我们页面  ->  Contact

> ps：  `product`里的可配置产品和下载产品比较特殊，不在catalog里，是单独扩展出来的。
+ 可配置产品  ->  ConfigurableProduct
+ 下载产品  ->  Downloadable


### ***注意注意***
**1. 不可以直接修改核心原代码，否则版本升级的时候会自动覆盖掉。**
**2. php文件的话用`plugin/preference/events`等方式来重写**
**3. phtml文件可以直接在自定义主题下重写**
**4. xml文件也可以直接在自定义主题/插件下边的`layout`里重写**




  