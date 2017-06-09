layout: first_layout
title: 20160918随笔
date: 2016-09-18 13:12:05
tags: 普通
comments: true
categories: Python

---

### 斐波那契数列
```python
# 递归
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```

### SimpleHttpServer 简单文件传输
+ python -m SimpleHTTPServer 
> 执行这个命令，就会在当前目录先启动一个简单的httpserver，在其他的服务器上通过ip+8000端口就可以下载到开启httpserver的服务器上当前目录下的文件,例如我在192.168.1.2的服务器上

```
▸ captcha_crack/
▾ data/
    simplehttpserver.txt
    test.py
```

data目录下执行python -m SimpleHTTPServer
在另外一台服务器shell下执行wget 192.168.1.2:8000/test.py就可以把test.py下载下来


### Nohup 后台执行任务
```
nohup python test.py &
```
像上面这样运行test.py脚本，这个进程会切换后台，即使在你的登录端退出，这个进程也会继续执行。

### Supervisor 进程管理
ubuntu下安装
```
apt-get install supervisor
```
为自己的进程配置一个conf文件，放在`/etc/supervisor/conf.d/`文件后缀为`.conf`

```supervisorctl -c /etc/supervisor.conf reload```

让你的配置文件生效(这里比较暴力，重新载入了supervisor的所有配置，更多方法前往[supervisor](http://supervisord.org/))


### Tmux 终端控制台
+ 窗口
+ 窗格
+ 会话
 
想了解更多情前往[tmux](http://tmux.github.io/)

### Ssh 远程登录
* 创建key 
 
```
ssh-keygen -t rsa -C 'your word'
```

想了解更多请前往[ssh中文手册](http://linux.chinaunix.net/techdoc/beginner/2010/01/12/1153509.shtml)