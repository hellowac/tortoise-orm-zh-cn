:tocdepth: 3

.. _contrib_fastapi:

================================
Tortoise-ORM FastAPI 集成
================================

**Tortoise-ORM FastAPI integration**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们有一个轻量级的集成工具 ``tortoise.contrib.fastapi``，其中包含一个类 ``RegisterTortoise``，可用于在生命周期上下文中设置和清理 Tortoise-ORM。

        FastAPI 基本上是 Starlette 和 Pydantic 的结合，但以一种非常特定的方式实现。

        请查看 :ref:`example_fastapi` 并浏览 :ref:`contrib_pydantic` 教程。

    .. md-tab-item:: 英文

        We have a lightweight integration util ``tortoise.contrib.fastapi`` which has a class ``RegisterTortoise`` that can be used to set/clean up Tortoise-ORM in lifespan context.

        FastAPI is basically Starlette & Pydantic, but in a very specific way.


        See the :ref:`example_fastapi` & have a look at the :ref:`contrib_pydantic` tutorials.

参考
=========

**Reference**

.. automodule:: tortoise.contrib.fastapi
    :members:
    :undoc-members:
    :show-inheritance:
