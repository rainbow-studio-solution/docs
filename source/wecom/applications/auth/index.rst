验证登陆
=================================

.. toctree::
   :maxdepth: 2

安装
---------------------------------

.. image:: img/install.png

-----------------

设置
---------------------------------

.. image:: img/setting.png

-----------------


设置 OAuth
---------------------------------

1. 确保使用域名访问，不要使用IP地址。如下图操作

.. image:: img/web_oauth_setting.png

-----------------

企微后台设置
---------------------------------

1. 登陆企业微信后台，“应用管理” → “自建” 。打开 “您的自建应用”，设置红框标识的区域
   
.. image:: img/wecom_setting.png

-----------------

2. 设置 网页授权及JS-SDK
  
.. image:: img/auth_settings.png

-----------------


3. 如何校验域名，如下图，下载校验文件

.. image:: img/verify_domain_name.png


3.1 在nginx配置文件中添加如下配置
  
.. code::

   location = /WW_verify_BQqOiwS117L9unq6.txt {
      default_type text/html;
      return 200 '校验文件的文本内容';
   }

3.2 如下图，点击“完成”按钮，完成域名校验

.. image:: img/verify_domain_name_end.png

-----------------

4. 设置扫码登陆

.. image:: img/scan_setting.png

-----------------

登陆页面展示
---------------------------------

.. image:: img/login.png

-----------------

.. image:: img/login_scan.png

-----------------

.. image:: img/login_onekey.png

-----------------