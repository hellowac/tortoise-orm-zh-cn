:tocdepth: 3

.. _contrib_blacksheep:

===================================
Tortoise-ORM BlackSheep 集成
===================================

**Tortoise-ORM BlackSheep integration**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们有一个轻量级的集成工具 ``tortoise.contrib.blacksheep``，其中包含一个函数 ``register_tortoise``，该函数在启动时设置 Tortoise-ORM，并在拆卸时进行清理。

        BlackSheep 是一个异步网络框架，用于使用 Python 构建基于事件的 web 应用程序。

        请查看 :ref:`example_blacksheep`，并参考 :ref:`contrib_pydantic` 教程。
    
    .. md-tab-item:: 英文

        We have a lightweight integration util ``tortoise.contrib.blacksheep`` which has a single function ``register_tortoise`` which sets up Tortoise-ORM on startup and cleans up on teardown.

        BlackSheep is an asynchronous web framework to build event based web applications with Python.


        See the :ref:`example_blacksheep` & have a look at the :ref:`contrib_pydantic` tutorials.

参考
=========

**Reference**

.. automodule:: tortoise.contrib.blacksheep
    :members:
    :undoc-members:
    :show-inheritance:
