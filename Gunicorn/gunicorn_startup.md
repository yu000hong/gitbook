# Gunicorn 启动流程


### 启动命令

$ gunicorn -w 4 myapp:app

我们来看看Gunicorn的安装脚本setup.py：

    entry_points="""
    [console_scripts]
    gunicorn=gunicorn.app.wsgiapp:run
    """

所以启动文件是：gunicorn/app/wsgiapp.py
启动方法是：run()

```
def run():
    """\
    The ``gunicorn`` command line runner for launching Gunicorn with
    generic WSGI applications.
    """
    from gunicorn.app.wsgiapp import WSGIApplication
    WSGIApplication("%(prog)s [OPTIONS] [APP_MODULE]").run()
```

继承关系
WSGIApplication -> Application -> BaseApplication

启动流程：
- BaseApplication.__init__()
    - BaseApplication.do_load_config()
- Application.run()
- BaseApplication.run()
    - Arbiter.__init__()
        - Arbiter.setup()
    - Artibter.run()
        - start()
        - manage_workers()
        - loop to manage workers（Master核心逻辑）
