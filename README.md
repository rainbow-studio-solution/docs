# Readthedocs 构建在线文档

## 安装以下软件包
```
pip3 install sphinx -i https://pypi.doubanio.com/simple
pip3 install sphinx-autobuild -i https://pypi.doubanio.com/simple
pip3 install sphinx_theme -i https://pypi.doubanio.com/simple
pip3 install sphinx_rtd_theme -i https://pypi.doubanio.com/simple
pip3 install recommonmark -i https://pypi.doubanio.com/simple
```

## 主题
[打开](https://sphinx-themes.org/)

## 启动 Sphinx
```
sphinx-autobuild source build/html

默认启动 8000 端口，在浏览器输入 http://127.0.0.1:8000 
```

## 导出 requirements.txt

```
pip freeze > requirements.txt
```