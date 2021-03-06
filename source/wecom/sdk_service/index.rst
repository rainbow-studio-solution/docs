部署SDK服务
=================================

.. toctree::
   :maxdepth: 2

用途
---------------------------------

1. 用于获取企业微信会话内容存档的聊天记录
   
2. 用于获取企业微信会话内容存档的聊天记录中的媒体文件

为什么使用FastAPI
---------------------------------

1. 社区朋友的测试结果，目前社区朋友已经这个问题提交给了odoo，[链接](https://github.com/odoo/odoo/issues/82623)：
   | odoo addons，依赖了多个三方库，这些库里面的某个用了（非pytyhon）c++库；它用的gcc/libstdc++ 库版本，和企业微信sdk提供的sdk.so编译使用的 gcc/libstdc++库不兼容。
    因为odoo addons机制先把这个依赖库(它用的gcc/libstdc++）加载了，然后才加载的 ctypes.CDLL(sdk_lib_path), 所以导致这里加载so时coredump了.

2. 经过2周的时间，也无法解决odoo加载c++库崩溃的问题，我已经崩溃了，干脆使用FastAPI封装企业微信会话内部存档的SDK，让Odoo访问 FastAPI,获取到相关信息。


安装SDK服务
---------------------------------

感谢 群友 NickCake 的支持

Debian系安装依赖
:::::::::::::::::::::::::::::::::

请使用APT安装依赖，否则无法正常启动SDK服务

.. code::

    sudo apt-get install python3-httptools python3-uvloop python3-uvicorn 


CentOS7 安装依赖
:::::::::::::::::::::::::::::::::

1.安装依赖包

.. code::

    sudo yum install epel-release -y
    sudo yum groupinstall "Development tools" -y
    sudo yum install zlib-devel bzip2-devel openssl-devel ncurses-devel zx-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel -y

2. 编译安装 python3.7.12

.. code::

    wget https://www.python.org/ftp/python/3.7.12/Python-3.7.12.tar.xz  --no-check-certificate
    tar xf Python-3.7.12.tar.xz
    cd Python-3.7.12
    ./configure --prefix=/usr/local --with-ensurepip=install --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib"
    sudo yum install libffi-devel -y 
    sudo make && sudo make altinstall

    sudo /usr/bin/python3 -m pip install --upgrade pip

3. 更新软连接
   
.. code::

    sudo rm /usr/bin/python3
    sudo ln -s /usr/local/bin/python3.7 /usr/bin/python3

添加wecomsdk服务文件
:::::::::::::::::::::::::::::::::

1. 在 / 创建 fastapi 文件夹

.. code::

    cd /
    mkdir fastapi

2. 复制 /wecom_sdk_service/ 下的所有文件到 /fastapi
3. 安装Python依赖

.. code::

    cd  /app
    # 安装需要sudo pip安装，否则会安装到非home目录
    sudo pip3 install -r  requirements.txt -i https://pypi.doubanio.com/simple 

4. 添加 wecomsdk.service 文件到 /lib/systemd/system/
5. 使用 whereis 或 which 命令查询Python3 和 uvicorn 的路径，添加服务的时候需要用到

.. code::

    whereis python3
    python3: /usr/bin/python3 /usr/bin/python3.7m /usr/bin/python3.7m-config /usr/bin/python3.7 /usr/bin/python3.7-config /usr/lib/python3 /usr/lib/python3.7 /etc/python3 /etc/python3.7 /usr/local/lib/python3.7 /usr/include/python3.7m /usr/include/python3.7 /usr/share/python3 /usr/share/man/man1/python3.1.gz
    whereis uvicorn
    uvicorn: /usr/local/bin/uvicorn

    python3的路径:   /usr/bin/python3
    uvicorn的路径:   /usr/local/bin/uvicorn


6. 在 /lib/systemd/system/wecomsdk.service 中添加以下内容：

.. code::

    [Unit]
    Description=Wecom Sdk Api
    After=network.target

    [Service]
    Type=simple
    WorkingDirectory=/fastapi
    Environment=FLAGS="app.main:app --host 0.0.0.0 --port 8000"
    ExecStart=python3的路径 uvicorn的路径 $FLAGS 
    StandardOutput=/var/log/wecom/wecom-server.log
    Restart=always
    RestartSec=6

    [Install]
    WantedBy=default.target

7. 启动服务
   
.. code::

    systemctl enable wecomsdk
    systemctl start wecomsdk

8. 查看日志
   
.. code::

    tail -f /var/log/wecom/wecom-server.log

`请求示例 <https://gitee.com/rainbowstudio/wecom/blob/14.0/wecom_msgaudit/models/wecom_chatdata.py>`_ 
---------------------------------

1. 获取聊天记录

.. code:: python    

    import requests
    import json

    ir_config = self.env["ir.config_parameter"].sudo()
    chatdata_url = ir_config.get_param(
        "wecom.msgaudit.msgaudit_sdk_url"
    ) + ir_config.get_param("wecom.msgaudit.msgaudit_chatdata_url")

    proxy = (
        True
        if ir_config.get_param("wecom.msgaudit_sdk_proxy") == "True"
        else False
    )

    headers = {"content-type": "application/json"}
    body = {
        "seq": max_seq_id, #从指定的seq开始拉取消息，注意的是返回的消息从seq+1开始返回，seq为之前接口返回的最大seq值。首次使用请使用seq:0
        "corpid": corpid,
        "secret": secret,
        "private_keys": 私钥列表,
    }
    if proxy:
        body.update(
            {"proxy": chatdata_url, "paswd": "odoo:odoo",}
        )
    r = requests.get(chatdata_url, data=json.dumps(body), headers=headers)
    chat_datas = r.json()

2. 获取媒体文件


.. code:: python    

    import requests
    import json

    ir_config = self.env["ir.config_parameter"].sudo()
    mediadata_url = ir_config.get_param(
        "wecom.msgaudit.msgaudit_sdk_url"
    ) + ir_config.get_param("wecom.msgaudit.msgaudit_mediadata_url")
    proxy = (
        True
        if ir_config.get_param("wecom.msgaudit_sdk_proxy") == "True"
        else False
    )

    headers = {"content-type": "application/json"}
    body = {
        "seq": 0, #为0即可
        "sdkfileid": 消息体内容中的sdkfileid信息,
        "corpid": corpid,
        "secret": secret,
        "private_keys": 私钥列表,
    }
    if proxy:
        body.update(
            {"proxy": mediadata_url, "paswd": "odoo:odoo",}
        )
    r = requests.get(mediadata_url, data=json.dumps(body), headers=headers)
    mediadata = r.json()

SDK错误码说明 
---------------------------------

.. table:: 

==========  ==============  ==============
返回值       说明             建议
==========  ==============  ==============
10000       请求参数错误     检查Init接口corpid、secret参数；检查GetChatData接口limit参数是否未填或大于1000；检查GetMediaData接口sdkfileid是否为空，indexbuf是否正常
10001       网络请求错误     检查是否网络有异常、波动；检查使用代理的情况下代理参数是否设置正确的用户名与密码                      
10002       数据解析失败     建议重试请求。若仍失败，可以反馈给企业微信进行查询，请提供sdk接口参数与调用时间点等信
10003       系统调用失败     GetMediaData调用失败，建议重试请求。若仍失败，可以反馈给企业微信进行查询，请提供sdk接口参数与调用时间点等信
10004       已废弃          目前不会返回此错误
10005       fileid错误      检查在GetMediaData接口传入的sdkfileid是否正
10006       解密失败        请检查是否先进行base64decode再进行rsa私钥解密，再进行DecryptMsg调用           
10007       已废弃          目前不会返回此错误码
10008       DecryptMsg错误  建议重试请求。若仍失败，可以反馈给企业微信进行查询，请提供sdk接口参数与调用时间点等信
10009       ip非法          请检查sdk访问外网的ip是否与管理端设置的可信ip匹配，若不匹配会返回此错误码                           
10010       请求的数据过期   用户欲拉取的数据已过期，仅支持近3天内的数据拉取
10011       ssl证书错误     使用openssl版本sdk，校验ssl证书失
==========  ==============  ==============