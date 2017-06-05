layout: Werkzeug
title: Werkzeug库简介
date: 2017-06-05 14:50:11
tag: 重点
---

### Werkzeug 库简介

Werkzeug 并不是一个框架, 它是一个WSGI工具集的库, 你可以通过它来创建自己的框架或者应用, Werkzeug是非常灵活的, 希望这篇文章对你有所帮助.

WSGI(Web Server Gateway Interface)是WEB服务器和web框架或web应用之间建立的一种简单通用的接口协议.

Werkzeug遵循WSGI的规范, 下面是一个简单的例子:
```python
#!/usr/bin/env python
# coding: utf-8

from werkzeug.wrappers import Request, Response


def my_app_version1(environ, start_response):
    """
    在没有werkzeug的帮助的话, 使用WSGI实现一个简单Web看起来是这样的
    :param environ: 包含了所有进来的信息
    :param start_response: 用来表明收到一个响应
    :return:
    """
    query_string = environ.get('QUERY_STRING')
    name = 'Man'
    if query_string:
        name = query_string.split('=')[1]
    start_response('200 OK', [('Content-Type', 'text/plain')])
    return 'Hello %s, This is My App Version 0.1' % name


def my_app_version2(environ, start_response):
    """
    使用了Werkzeug 所提供的Request和Response, 
    你就不需要自己去直接去处理提交上来的请求(request)和响应(response)
    :param environ:
    :param start_response:
    :return:
    """
    request = Request(environ)
    response = Response('Hello %s, This is My App Version 0.2' %
                        request.args.get('name', 'Man'))
    return response(environ, start_response)


if __name__ == '__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 5011, my_app_version1)
    # run_simple('localhost', 5012, my_app_version2)
```

#### 用Werkzeug构建一个简单的应用

```python
class SimpleFlask(object):

    def __init__(self, name):
        self.name = name
        self.rv = Response

    def dispatch_request(self, request):
        return self.rv('Hello %s, This is SimpleFlask' %
                       request.args.get('name', 'Man'))

    def wsgi_app(self, environ, start_reponse):
        request = Request(environ)
        response = self.dispatch_request(request)
        return response(environ, start_reponse)

    def __call__(self, environ, start_response):
        return self.wsgi_app(environ, start_response)


def create_app():
    app = SimpleFlask(__name__)
    return app


if __name__ == '__main__':
    from werkzeug.serving import run_simple
    simple_app = create_app()
    run_simple('localhost', 5012, simple_app)
```
