:tocdepth: 3

.. _connections:

===========
连接
===========

**Connections**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        本文档描述了如何访问底层连接对象（:ref:`BaseDBAsyncClient<base_db_client>`），该对象是作为传递给 :meth:`Tortoise.init<tortoise.Tortoise.init>` 调用的一部分定义的数据库配置中的别名。

        下面是一个简单的代码片段，展示了如何访问该接口：

        .. code-block:: python3

            # connections 是 ConnectionHandler 类的单例实例，作为
            # 访问所有连接管理 API 的入口点。
            from tortoise import connections


            # 假设这是使用的 Tortoise 配置
            await Tortoise.init(
                {
                    "connections": {
                        "default": {
                            "engine": "tortoise.backends.sqlite",
                            "credentials": {"file_path": "example.sqlite3"},
                        }
                    },
                    "apps": {
                        "events": {"models": ["__main__"], "default_connection": "default"}
                    },
                }
            )

            conn: BaseDBAsyncClient = connections.get("default")
            try:
                await conn.execute_query('SELECT * FROM "event"')
            except OperationalError:
                print("预期会失败")

        .. important::

            :ref:`tortoise.connection.ConnectionHandler<connection_handler>` 类是以单例模式实现的，因此当 ORM 初始化时，会自动创建该类的单例实例 ``tortoise.connection.connections``，并在应用的生命周期内保持在内存中。在运行时尝试修改或覆盖其行为是有风险的，不建议这样做。

        请参阅 :ref:`此示例<example_two_databases>` 以详细演示如何在实践中使用此 API。
    
    .. md-tab-item:: 英文

        This document describes how to access the underlying connection object (:ref:`BaseDBAsyncClient<base_db_client>`) for the aliases defined as part of the DB config passed to the :meth:`Tortoise.init<tortoise.Tortoise.init>` call.

        Below is a simple code snippet which shows how the interface can be accessed:

        .. code-block::  python3

            # connections is a singleton instance of the ConnectionHandler class and serves as the
            # entrypoint to access all connection management APIs.
            from tortoise import connections


            # Assume that this is the Tortoise configuration used
            await Tortoise.init(
                {
                    "connections": {
                        "default": {
                            "engine": "tortoise.backends.sqlite",
                            "credentials": {"file_path": "example.sqlite3"},
                        }
                    },
                    "apps": {
                        "events": {"models": ["__main__"], "default_connection": "default"}
                    },
                }
            )

            conn: BaseDBAsyncClient = connections.get("default")
            try:
                await conn.execute_query('SELECT * FROM "event"')
            except OperationalError:
                print("Expected it to fail")

        .. important::
            
            The :ref:`tortoise.connection.ConnectionHandler<connection_handler>` class has been implemented with the singleton pattern in mind and so when the ORM initializes, a singleton instance of this class ``tortoise.connection.connections`` is created automatically and lives in memory up until the lifetime of the app. Any attempt to modify or override its behaviour at runtime is risky and not recommended.


        Please refer to :ref:`this example<example_two_databases>` for a detailed demonstration of how this API can be used in practice.


API Reference
===========

.. _connection_handler:

.. automodule:: tortoise.connection
    :members:
    :undoc-members: