企微HRM
=================================

.. toctree::
   :maxdepth: 2

隐藏“员工菜单”
---------------------------------

“企微HRM”的功能在“员工”基础上增加了企业微信的相关功能，故建议隐藏“员工菜单”。如下图操作：

.. image:: img/menu_hide.png

---------------------------------

通讯簿同步Block列表
---------------------------------

同步企微通讯簿时，忽略Block列表中的成员。如下图操作：

1. 如下图操作，打开菜单：
   
.. image:: img/block1.png

-----------------

2. 如下图操作，添加不同步的的成员：
   
.. image:: img/block2.png

-----------------

使用向导同步通讯录
---------------------------------

1. 开始同步通讯录
   
.. image:: img/wizard_hrm_syncing.png

-----------------

2. 启动
   
.. image:: img/wizard_hrm_sync_start.png

-----------------

3. 结束同步
   
.. image:: img/wizard_hrm_sync_end.png

-----------------

手动绑定企微成员
---------------------------------

由于HR存在同企微组织架构的成员，只是尚未绑定。在 使用向导同步通讯录 功能之前，需要手动绑定企微成员。

如下图操作：

1. 在“企微HRM” 的员工里，切换 List 视图，或者打开需要绑定的员工。

.. image:: img/binding_members1.png

-----------------

.. image:: img/binding_members2.png

-----------------

2. 在弹出的窗口中，输入企微成员 userid，点击“绑定”按钮，如下图操作：

.. image:: img/binding_members3.png

-----------------

3. 完成手工绑定：

.. image:: img/binding_members4.png

-----------------


使用向导同步系统用户
---------------------------------

1. 设置允许员工批量生成系统用户，如下图设置，点击保存：
   
.. image:: img/allow_user_sync.png

----------------- 

2. 开始将带有企业微信标识的员工批量生成系统用户
   
.. image:: img/wizard_user_syncing.png

-----------------

3. 启动
   
.. image:: img/wizard_user_sync_start.png

-----------------

4. 结束同步

.. image:: img/wizard_user_sync_end.png

-----------------

单个员工生成系统用户
---------------------------------

若场景需求，不允许从员工批量生成系统用户，可以在“设置”→“通讯录同步设置”→“应用配置” 中将 “contacts_sync_user_enabled”的值 改为 “False”。如下面的步骤进行操作：

.. image:: img/employee2user.png

-----------------

.. image:: img/employee2user2.png

-----------------


设置企业微信系统用户
---------------------------------

从HRM生成的系统用户默认为 “Share User”，“Share User”为具有受限访问权限的外部用户，仅出于共享数据的目的而创建，可以访问“website”和“portal”，不能访问“web”。

将HRM生成的系统用户若需要访问“web”，可以安装下列步骤操作：

1. 菜单路径：“设置” → “用户和公司” → “用户”，如下图:

.. image:: img/user_delete_filter.png

-----------------

2. 变更用户类型：

.. image:: img/user_change_type.png

-----------------

.. image:: img/user_change_type2.png

-----------------