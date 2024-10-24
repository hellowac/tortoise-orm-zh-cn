:tocdepth: 3

.. _contrib_quart:

==============================
Tortoise-ORM Quart 集成
==============================

**Tortoise-ORM Quart integration**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们有一个轻量级的集成工具 ``tortoise.contrib.quart``，其中包含一个函数 ``register_tortoise``，该函数在启动时设置 Tortoise-ORM，并在拆卸时进行清理。

        请注意，模块路径不能是 ``__main__`` ，因为这会根据启动点而变化。希望能够直接从 ASGI 运行器启动 Quart 服务，因此所有路径都需要是明确的。

        请查看 :ref:`example_quart`。

    .. md-tab-item:: 英文

        We have a lightweight integration util ``tortoise.contrib.quart`` which has a single function ``register_tortoise`` which sets up Tortoise-ORM on startup and cleans up on teardown.

        Note that the modules path can not be ``__main__`` as that changes depending on the launch point. One wants to be able to launch a Quart service from the ASGI runner directly, so all paths need to be explicit.

        See the :ref:`example_quart`

用法
=====

**Usage**

.. code-block:: sh

    QUART_APP=main quart
        ...
        Commands:
          generate-schemas  Populate DB with Tortoise-ORM schemas.
          run               Start and run a development server.
          shell             Open a shell within the app context.

    # To generate schemas
    QUART_APP=main quart generate-schemas

    # To run
    QUART_APP=main quart run


参考
=========

**Reference**

.. automodule:: tortoise.contrib.quart
    :members:
    :undoc-members:
    :show-inheritance:
