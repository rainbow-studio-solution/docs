使用 Docker 部署
=================================

.. toctree::
   :maxdepth: 2

1. 启动一个Postgresql 服务器

    | docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:14

2. 拉取 wecom_for_odoo镜像

    | docker pull rainbowstudiosolution/wecom_for_odoo:14

3. 启动企业微信Odoo实列

    | docker run -v odoo.conf路径:/etc/odoo -v 模块路径:/mnt/extra-addons -p 8069:8069 --name wecom_for_odoo --link db:db -t rainbowstudiosolution/wecom_for_odoo:14