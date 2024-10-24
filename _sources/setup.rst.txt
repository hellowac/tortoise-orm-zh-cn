:tocdepth: 3

======
启动
======

**Set up**

.. _init_app:

初始化app
========

**Init app**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        在您定义所有模型后，Tortoise 需要您初始化它们，以便创建模型之间的反向关系，并将您的数据库客户端与相应的模型匹配。

        您可以这样做：

        .. code-block:: python3

            from tortoise import Tortoise

            async def init():
                # 在这里我们使用文件 "db.sqlite3" 创建一个 SQLite 数据库
                # 同时指定应用名称为 "models"
                # 其中包含来自 "app.models" 的模型
                await Tortoise.init(
                    db_url='sqlite://db.sqlite3',
                    modules={'models': ['app.models']}
                )
                # 生成模式
                await Tortoise.generate_schemas()
            
            
        在这里，我们创建一个 SQLite 数据库客户端的连接，然后发现并初始化模型。

        ``generate_schema`` 在空数据库上生成模式，您不应该在每次应用初始化时运行它，可能只需运行一次，最好在主代码之外。
        在生成模式时，您还可以选择将 ``safe`` 参数设置为 ``True``，这将仅在表不存在时插入表。

        如果您在 ``app.models`` 模块中定义变量 ``__models__`` （或在您指定加载模型的其他地方）， ``generate_schema`` 将使用该列表，而不是自动为您查找模型。

    .. md-tab-item:: 英文

        After you defined all your models, tortoise needs you to init them, in order to create backward relations between models and match your db client with appropriate models.

        You can do it like this:

        .. code-block:: python3

            from tortoise import Tortoise

            async def init():
                # Here we create a SQLite DB using file "db.sqlite3"
                #  also specify the app name of "models"
                #  which contain models from "app.models"
                await Tortoise.init(
                    db_url='sqlite://db.sqlite3',
                    modules={'models': ['app.models']}
                )
                # Generate the schema
                await Tortoise.generate_schemas()
            
            
        Here we create connection to SQLite database client and then we discover & initialize models.

        ``generate_schema`` generates schema on empty database, you shouldn't run it on every app init, run it just once, maybe out of your main code.
        There is also the option when generating the schemas to set the ``safe`` parameter to ``True`` which will only insert the tables if they don't already exist.

        If you define the variable ``__models__`` in the ``app.models`` module (or wherever you specify to load your models from), ``generate_schema`` will use that list, rather than automatically finding models for you.

.. _cleaningup:

清理的重要性
=============================

**The Importance of cleaning up**

.. md-tab-set::
    
    .. md-tab-item:: 中文
            
        Tortoise ORM 将保持与外部数据库的连接。作为一个 ``asyncio`` Python 库，它需要正确关闭连接，否则 Python 解释器可能仍会等待这些连接的完成。

        为确保连接被关闭，请确保调用 ``Tortoise.close_connections()``:

        .. code-block:: python3

            await Tortoise.close_connections()

        小帮助函数 ``tortoise.run_async()`` 将确保连接被关闭。

    .. md-tab-item:: 英文

        Tortoise ORM will keep connections open to external Databases. As an ``asyncio`` Python library, it needs to have the connections closed properly or the Python interpreter may still wait for the completion of said connections.

        To ensure connections are closed please ensure that ``Tortoise.close_connections()`` is called:

        .. code-block:: python3

            await Tortoise.close_connections()

        The small helper function ``tortoise.run_async()`` will ensure that connections are closed.

参考
=========

**Reference**

.. automodule:: tortoise
    :members:
    :undoc-members:

