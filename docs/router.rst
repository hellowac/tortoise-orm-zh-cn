:tocdepth: 4

.. _router:

======
路由
======

**Router**

.. md-tab-set::
    
    .. md-tab-item:: 中文
        
        使用多个数据库的最简单方法是设置数据库路由方案。默认的路由方案确保对象始终 “粘附(sticky)” 到它们原始的数据库（即，从 foo 数据库检索的对象将被保存到同一数据库）。默认的路由方案确保如果没有指定数据库，所有查询都将回退到默认数据库。

    .. md-tab-item:: 英文

        The easiest way to use multiple databases is to set up a database routing scheme. The default routing scheme ensures that objects remain 'sticky' to their original database (i.e., an object retrieved from the foo database will be saved on the same database). The default routing scheme ensures that if a database isn't specified, all queries fall back to the default database.

用法
=====

**Usage**

定义路由
-------------

**Define Router**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        定义一个路由器很简单，你只需要编写一个包含 `db_for_read` 和 `db_for_write` 方法的类。

        .. code-block:: python3

            class Router:
                def db_for_read(self, model: Type[Model]):
                    return "slave"

                def db_for_write(self, model: Type[Model]):
                    return "master"

        这两个方法返回在配置中定义的连接字符串。
    
    .. md-tab-item:: 英文

        Define a router is simple, you need just write a class that has `db_for_read` and `db_for_write` methods.

        .. code-block:: python3

            class Router:
                def db_for_read(self, model: Type[Model]):
                    return "slave"

                def db_for_write(self, model: Type[Model]):
                    return "master"

        The two methods return a connection string defined in configuration.

配置路由
-------------

**Config Router**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        只需将其放入 Tortoise 的配置中或在 `Tortoise.init` 方法中。

        .. code-block:: python3

            config = {
                "connections": {"master": "sqlite:///tmp/test.db", "slave": "sqlite:///tmp/test.db"},
                "apps": {
                    "models": {
                        "models": ["__main__"],
                        "default_connection": "master",
                    }
                },
                "routers": ["path.Router"],
                "use_tz": False,
                "timezone": "UTC",
            }
            await Tortoise.init(config=config)
            # 或者
            routers = config.pop('routers')
            await Tortoise.init(config=config, routers=routers)

        之后，所有 `select` 操作将使用 `slave` 连接，所有 `create/update/delete` 操作将使用 `master` 连接。
    
    .. md-tab-item:: 英文

        Just put it in configuration of tortoise or in `Tortoise.init` method.

        .. code-block:: python3

            config = {
                "connections": {"master": "sqlite:///tmp/test.db", "slave": "sqlite:///tmp/test.db"},
                "apps": {
                    "models": {
                        "models": ["__main__"],
                        "default_connection": "master",
                    }
                },
                "routers": ["path.Router"],
                "use_tz": False,
                "timezone": "UTC",
            }
            await Tortoise.init(config=config)
            # or
            routers = config.pop('routers')
            await Tortoise.init(config=config, routers=routers)

        After that, all `select` operations will use `slave` connection, all `create/update/delete` operations will use `master` connection.
