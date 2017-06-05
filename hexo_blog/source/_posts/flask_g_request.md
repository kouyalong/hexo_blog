layout: flask_g_request_layout
title: Flask学习笔记第一篇之上下文
date: 2017-06-02 13:26:05
tags: 重点
comments: true
---

本文只是记录个人学习Flask上下文的一些理解. 关于上下文的理解和更多的信息, 可以参考最下方的参考文献!
Flask 中有两种上下文 请求上下文和应用上下文. 

#### Werkzeug 实现的 Local Stack 和 Local Proxy

在了解Flask 上下文之前, 必须得先了解Werkzeug.local.Local

Werkzeug 实现了一个Local类, 类似于ThreadLocal.

首先我们看看ThreadLocal. ThreadLocal则是一种特殊的对象, 它的"状态"对线程隔离——也就是说每个线程对一个ThreadLocal对象的修改都不会影响其他线程. 

ThreadLocal的内部实现:

```python
class _local(object):
    """ Thread-local data """
    def __delattr__(self, name): # real signature unknown; restored from __doc__
        """ x.__delattr__('name') <==> del x.name """
        pass

    def __getattribute__(self, name): # real signature unknown; restored from __doc__
        """ x.__getattribute__('name') <==> x.name """
        pass

    def __init__(self, *args, **kwargs): # real signature unknown
        pass

    @staticmethod # known case of __new__
    def __new__(S, *more): # real signature unknown; restored from __doc__
        """ T.__new__(S, ...) -> a new object with type S, a subtype of T """
        pass

    def __setattr__(self, name, value): # real signature unknown; restored from __doc__
        """ x.__setattr__('name', value) <==> x.name = value """
        pass

```

ThreadLocal Demo:

```python
#!/usr/bin/env python
# coding: utf-8

import threading


thread_local = threading.local()
thread_local.main_id = 0

print thread_local.main_id     # 主线程里面, 打印结果为0


class AppThread(threading.Thread):
    num = 0

    def set_num(self, num):
        self.num = num

    def run(self):
        thread_local.main_id = self.num
        print thread_local.main_id      # 在这个线程里面, 修改mian_id值


app_thread = AppThread()
app_thread.set_num(4)
app_thread.start()
print thread_local.main_id      # 主线程 main_id 没有改变

```
Werkzeug.local.Local类
* Werkzeug.local.Local会在Greenlet可用的情况下优先使用Greenlet的ID而不是线程ID以支持Gevent或Eventlet的调度; ThreadLocal只支持多线程调度
* Werkzeug.local.Local实现的__release_local__的方法, 可以被release_local函数释放掉当前线程下的状态, 源码中有Example

```python
def release_local(local):
    """Releases the contents of the local for the current context.
    This makes it possible to use locals without a manager.

    Example::

        >>> loc = Local()
        >>> loc.foo = 42
        >>> release_local(loc)
        >>> hasattr(loc, 'foo')
        False

    With this function one can release :class:`Local` objects as well
    as :class:`LocalStack` objects.  However it is not possible to
    release data held by proxies that way, one always has to retain
    a reference to the underlying local object in order to be able
    to release it.

    .. versionadded:: 0.6.1
    """
    local.__release_local__()


class Local(object):
    __slots__ = ('__storage__', '__ident_func__')

    def __init__(self):
        object.__setattr__(self, '__storage__', {})
        object.__setattr__(self, '__ident_func__', get_ident)

    def __iter__(self):
        return iter(self.__storage__.items())

    def __call__(self, proxy):
        """Create a proxy for a name."""
        return LocalProxy(self, proxy)

    def __release_local__(self):
        self.__storage__.pop(self.__ident_func__(), None)

    def __getattr__(self, name):
        try:
            return self.__storage__[self.__ident_func__()][name]
        except KeyError:
            raise AttributeError(name)

    def __setattr__(self, name, value):
        ident = self.__ident_func__()
        storage = self.__storage__
        try:
            storage[ident][name] = value
        except KeyError:
            storage[ident] = {name: value}

    def __delattr__(self, name):
        try:
            del self.__storage__[self.__ident_func__()][name]
        except KeyError:
            raise AttributeError(name)
```

接下来我们来看看Werkzeug实现的LocalStack 和 LocalProxy

LocalStack是用Werkzeug.local.Local实现的stack结构, 实现了pop, push以及top(快速拿到栈顶元素)等方法, 这些所有的操作都只在本线程可见.
LocaLStack也可以被release_local函数释放掉当前线程下的状态(它也实现了__release_local__方法)
```python
class LocalStack(object):

    """This class works similar to a :class:`Local` but keeps a stack
    of objects instead.  This is best explained with an example::

        >>> ls = LocalStack()
        >>> ls.push(42)
        >>> ls.top
        42
        >>> ls.push(23)
        >>> ls.top
        23
        >>> ls.pop()
        23
        >>> ls.top
        42

    They can be force released by using a :class:`LocalManager` or with
    the :func:`release_local` function but the correct way is to pop the
    item from the stack after using.  When the stack is empty it will
    no longer be bound to the current context (and as such released).

    By calling the stack without arguments it returns a proxy that resolves to
    the topmost item on the stack.

    .. versionadded:: 0.6.1
    """

    def __init__(self):
        self._local = Local()

    def __release_local__(self):
        self._local.__release_local__()

    def _get__ident_func__(self):
        return self._local.__ident_func__

    def _set__ident_func__(self, value):
        object.__setattr__(self._local, '__ident_func__', value)
    __ident_func__ = property(_get__ident_func__, _set__ident_func__)
    del _get__ident_func__, _set__ident_func__

    def __call__(self):
        def _lookup():
            rv = self.top
            if rv is None:
                raise RuntimeError('object unbound')
            return rv
        return LocalProxy(_lookup)

    def push(self, obj):
        """Pushes a new item to the stack"""
        rv = getattr(self._local, 'stack', None)
        if rv is None:
            self._local.stack = rv = []
        rv.append(obj)
        return rv

    def pop(self):
        """Removes the topmost item from the stack, will return the
        old value or `None` if the stack was already empty.
        """
        stack = getattr(self._local, 'stack', None)
        if stack is None:
            return None
        elif len(stack) == 1:
            release_local(self._local)
            return stack[-1]
        else:
            return stack.pop()

    @property
    def top(self):
        """The topmost item on the stack.  If the stack is empty,
        `None` is returned.
        """
        try:
            return self._local.stack[-1]
        except (AttributeError, IndexError):
            return None
```

LocalProxy则是一个典型的代理模式实现, LocalProxy接受一个callable的参数(比如一个函数)和一个string(可选), 这个参数被调用后本身应该是一个Werkzeug.local.Locald对象.
把这个对LocalProxy对象的所有操作, 包括属性访问, 方法调用, 甚至是二元操作都会转发到Werkzeug.local.Locald对象上, 并且设置这个这个对象的__name__=string

```python
@implements_bool
class LocalProxy(object):

    """Acts as a proxy for a werkzeug local.  Forwards all operations to
    a proxied object.  The only operations not supported for forwarding
    are right handed operands and any kind of assignment.

    Example usage::

        from werkzeug.local import Local
        l = Local()

        # these are proxies
        request = l('request')
        user = l('user')


        from werkzeug.local import LocalStack
        _response_local = LocalStack()

        # this is a proxy
        response = _response_local()

    Whenever something is bound to l.user / l.request the proxy objects
    will forward all operations.  If no object is bound a :exc:`RuntimeError`
    will be raised.

    To create proxies to :class:`Local` or :class:`LocalStack` objects,
    call the object as shown above.  If you want to have a proxy to an
    object looked up by a function, you can (as of Werkzeug 0.6.1) pass
    a function to the :class:`LocalProxy` constructor::

        session = LocalProxy(lambda: get_current_request().session)

    .. versionchanged:: 0.6.1
       The class can be instanciated with a callable as well now.
    """
```

#### wsgi_app

* 第一步：生成request请求对象和请求上下文环境
* 第二步：请求进入预处理，错误处理及请求转发到响应的过程
* 第三步：请求分发 dispatch_request
* 第四步：返回到wsgi_app内部

![示例图](https://martin-upload.b0.upaiyun.com/web/2017/06/bf02aa078b9ba72d9fcda9b3b45125fd.jpeg)

```python
    def wsgi_app(self, environ, start_response):
        """The actual WSGI application.  This is not implemented in
        `__call__` so that middlewares can be applied without losing a
        reference to the class.  So instead of doing this::

            app = MyMiddleware(app)

        It's a better idea to do this instead::

            app.wsgi_app = MyMiddleware(app.wsgi_app)

        Then you still have the original application object around and
        can continue to call methods on it.

        .. versionchanged:: 0.7
           The behavior of the before and after request callbacks was changed
           under error conditions and a new callback was added that will
           always execute at the end of the request, independent on if an
           error occurred or not.  See :ref:`callbacks-and-errors`.

        :param environ: a WSGI environment
        :param start_response: a callable accepting a status code,
                               a list of headers and an optional
                               exception context to start the response
        """
        ctx = self.request_context(environ)
        ctx.push()
        error = None
        try:
            try:
                response = self.full_dispatch_request()
            except Exception as e:
                error = e
                response = self.make_response(self.handle_exception(e))
            return response(environ, start_response)
        finally:
            if self.should_ignore_error(error):
                error = None
            ctx.auto_pop(error)

    def __call__(self, environ, start_response):
        """Shortcut for :attr:`wsgi_app`."""
        return self.wsgi_app(environ, start_response)
```

#### 应用上下文 AppContext

应用上下文会在合适的时间创建和销毁, 它不会在线程中移动, 并且也不会在不同的请求之间共享, 所以它一般用来存储数据库连接信息或者别的东西的最佳位置.
当`app = Flask(__name__)`构造出一个Flask App的时候, AppContext并不会自动推入到_app_ctx_stack中, 
如果你使用Flask-SQLAlchemy, 需要写一个离线脚本时候, 你就会知道这其中的坑(RuntimeError). 
解决的办法是: 在运行离线脚本之前, 先将App的AppContext PUSH到_app_ctx_stack中, `ctx = app.app_context()  ctx.push()`
但是当我们正常跑Flask服务的时候, 是不需要手动push. 推入部分的逻辑有这样一条：如果发现 _app_ctx_stack 为空，则隐式地推入一个AppContext。


```python
flask.globals.py


def _lookup_app_object(name):
    top = _app_ctx_stack.top
    if top is None:
        raise RuntimeError(_app_ctx_err_msg)
    return getattr(top, name)
    

def _find_app():
    top = _app_ctx_stack.top
    if top is None:
        raise RuntimeError(_app_ctx_err_msg)
    return top.app

_app_ctx_stack = LocalStack()
current_app = LocalProxy(_find_app)
g = LocalProxy(partial(_lookup_app_object, 'g'))
```

```python
class AppContext(object):
    """The application context binds an application object implicitly
    to the current thread or greenlet, similar to how the
    :class:`RequestContext` binds request information.  The application
    context is also implicitly created if a request context is created
    but the application is not on top of the individual application
    context.
    """

    def __init__(self, app):
        self.app = app
        self.url_adapter = app.create_url_adapter(None)
        self.g = app.app_ctx_globals_class()

        # Like request context, app contexts can be pushed multiple times
        # but there a basic "refcount" is enough to track them.
        self._refcnt = 0
```


#### 请求上下文 RequestContext
request和session都属于请求上下文
request 针对的是http请求作为对象
session 针对的是更多是用户信息作为对象


```python


def _lookup_req_object(name):
    top = _request_ctx_stack.top
    if top is None:
        raise RuntimeError(_request_ctx_err_msg)
    return getattr(top, name)

_request_ctx_stack = LocalStack()
request = LocalProxy(partial(_lookup_req_object, 'request'))
session = LocalProxy(partial(_lookup_req_object, 'session'))
```

```python
class RequestContext(object):
    """The request context contains all request relevant information.  It is
    created at the beginning of the request and pushed to the
    `_request_ctx_stack` and removed at the end of it.  It will create the
    URL adapter and request object for the WSGI environment provided.

    Do not attempt to use this class directly, instead use
    :meth:`~flask.Flask.test_request_context` and
    :meth:`~flask.Flask.request_context` to create this object.

    When the request context is popped, it will evaluate all the
    functions registered on the application for teardown execution
    (:meth:`~flask.Flask.teardown_request`).

    The request context is automatically popped at the end of the request
    for you.  In debug mode the request context is kept around if
    exceptions happen so that interactive debuggers have a chance to
    introspect the data.  With 0.4 this can also be forced for requests
    that did not fail and outside of ``DEBUG`` mode.  By setting
    ``'flask._preserve_context'`` to ``True`` on the WSGI environment the
    context will not pop itself at the end of the request.  This is used by
    the :meth:`~flask.Flask.test_client` for example to implement the
    deferred cleanup functionality.

    You might find this helpful for unittests where you need the
    information from the context local around for a little longer.  Make
    sure to properly :meth:`~werkzeug.LocalStack.pop` the stack yourself in
    that situation, otherwise your unittests will leak memory.
    """

    def __init__(self, app, environ, request=None):
        self.app = app
        if request is None:
            request = app.request_class(environ)
        self.request = request
        self.url_adapter = app.create_url_adapter(self.request)
        self.flashes = None
        self.session = None

        # Request contexts can be pushed multiple times and interleaved with
        # other request contexts.  Now only if the last level is popped we
        # get rid of them.  Additionally if an application context is missing
        # one is created implicitly so for each level we add this information
        self._implicit_app_ctx_stack = []

        # indicator if the context was preserved.  Next time another context
        # is pushed the preserved context is popped.
        self.preserved = False

        # remembers the exception for pop if there is one in case the context
        # preservation kicks in.
        self._preserved_exc = None

        # Functions that should be executed after the request on the response
        # object.  These will be called before the regular "after_request"
        # functions.
        self._after_request_functions = []

        self.match_request()
```

##### 参考文献
* [Flask 应用上下文](http://www.pythondoc.com/flask/appcontext.html)
* [Flask 请求上下文](http://www.pythondoc.com/flask/reqcontext.html)
* [Flask 的 Context 机制](https://blog.tonyseek.com/post/the-context-mechanism-of-flask/)
* [Flask的Context(上下文)学习笔记](http://www.jianshu.com/p/7a7efbb7205f)
* [flask 源码解析：上下文](https://segmentfault.com/a/1190000008383197)