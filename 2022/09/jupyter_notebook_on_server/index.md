# 服务器部署JupyterLab以供远程访问



在服务器上部署JupyterLab供远程访问并Coding

<!--more-->

可能是当前全网关于访问服务器JupyterLab比较新的版本了，jupyter notebook则类似

「**备注**」**This is not the multi-user server you are looking for.** If you want a multi-user server, the official solution is [JupyterHub](https://jupyterhub.readthedocs.io/en/latest/).

## 前期准备

OS：Ubuntu Server 20.04

```bash
# Installs the package globally in your python installation, i.e. for all users.
sudo pip install jupyterlab
sudo pip install jupyter notebook
```

`sudo pip` 的方式以后服务器新建用户就不用再配置环境了，不然针对某个用户装的`jupyterlab`或`jupyter notebook`还需要加入`PATH`

## 各种配置

### 设置密码登录

notebook 5.0版本开始，可以通过以下命令直接生成密码

```bash
jupyter lab password

Enter password:  ****  # 以后可直接通过这个密码 login
Verify password: ****
[NotebookPasswordApp] Wrote hashed password to /Users/you/.jupyter/jupyter_server_config.json

```

密码会保存至 `jupyter_server_config.json`  文件

官方参考路径：

- Windows: `C:\Users\USERNAME\.jupyter\jupyter_notebook_config.py`
- OS X: `/Users/USERNAME/.jupyter/jupyter_notebook_config.py`
- Linux: `/home/USERNAME/.jupyter/jupyter_notebook_config.py`

### 其他一些配置

```json
{
  "ServerApp": {
    "ip":"*"
    ,"port":8888
    ,"open_browser":false
    ,"password": "上文自动生成的部分"
  }
}
```

端口默认是8888，不管如何都需要确认下服务器防火墙端口是否开放

## 启动与关闭

通过以下命令在后台运行服务，ssh窗口关闭后仍能正常访问

```bash
nohup jupyter-lab &
```

浏览器通过 `服务器公网IP:端口`（如，12.456.78.321:8888）的形式访问

`ps -aux | grep jupyter` 找到 jupyter 服务的pid

使用 `kill -9 pid`方式关闭运行中的jupyter服务 

## Reference

[Running a notebook server - Jupyter Notebook 6.4.12 documentation](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html)

[Linux服务器上创建新用户_攻城狮Bell的博客-CSDN博客_linux服务器创建用户](https://blog.csdn.net/weixin_37182342/article/details/80298464)

[如何访问服务器的 Jupyter notebook](https://zhuanlan.zhihu.com/p/69869583)

[搭建Jupyter Notebook远程云服务器](https://zhuanlan.zhihu.com/p/44405596)

[使用nohup 和 & 后台运行jupyter notebook程序，查看、kill 进程_Donald Su的博客-CSDN博客_查看jupyter进程](https://blog.csdn.net/donaldsy/article/details/96350061)


<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">
