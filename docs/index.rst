============
Tortoise ORM
============

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        Tortoise ORM 是一个易于使用的 ``asyncio`` ORM *(对象关系映射器)*，受 Django 启发。

        Tortoise ORM 的设计考虑了关系，并对优秀且流行的 Django ORM 表示钦佩。
        它的设计中铭刻着您不仅在处理表格，而是在处理关系数据。

        .. note::
        
            Tortoise ORM 是一个年轻的项目，预计会有重大变更。
            我们维护一个 :ref:`changelog`，可能的破坏性更改会被清晰记录。

        源代码和问题追踪器可在 `<https://github.com/tortoise/tortoise-orm/>`_ 获得。

        Tortoise ORM 支持 CPython >= 3.8，适用于 SQLite、MySQL 和 PostgreSQL。

    .. md-tab-item:: 英文

        Tortoise ORM is an easy-to-use ``asyncio`` ORM *(Object Relational Mapper)* inspired by Django.

        Tortoise ORM was built with relations in mind and admiration for the excellent and popular Django ORM.
        It's engraved in it's design that you are working not with just tables, you work with relational data.

        .. note::

            Tortoise ORM is young project and breaking changes are to be expected.
            We keep a :ref:`changelog` and it will have possible breakage clearly documented.

        Source & issue trackers are available at `<https://github.com/tortoise/tortoise-orm/>`_

        Tortoise ORM is supported on CPython >= 3.8 for SQLite, MySQL and PostgreSQL.

引言
============

**Introduction**

为什么要构建 Tortoise ORM？
---------------------------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Python 已有许多现存且成熟的 ORM，不幸的是，它们的设计与 I/O 处理的对立范式相悖。
        ``asyncio`` 是一种相对较新的技术，具有不同的并发模型，最大的变化在于 I/O 的处理方式。

        然而，Tortoise ORM 并不是构建 ``asyncio`` ORM 的第一次尝试。许多开发者尝试将同步 Python ORM 映射到异步世界，但最初的尝试并没有一个干净的 API。

        因此，我们开始了 Tortoise ORM。

        Tortoise ORM 的设计旨在功能性与熟悉性兼备，以便于希望切换到 ``asyncio`` 的开发者的迁移。

        与其他 Python ORM 相比，它的性能也表现良好，与 Pony ORM 交替领先：

        .. image:: ORM_Perf.png
            :target: https://github.com/tortoise/orm-benchmarks

    .. md-tab-item:: 英文

        **Why was Tortoise ORM built?**

        Python has many existing and mature ORMs, unfortunately they are designed with an opposing paradigm of how I/O gets processed.
        ``asyncio`` is relatively new technology that has a different concurrency model, and the largest change is regarding how I/O is handled.

        However, Tortoise ORM is not first attempt of building ``asyncio`` ORM, there are many cases of developers attempting to map synchronous python ORMs to the async world, initial attempts did not have a clean API.

        Hence we started Tortoise ORM.

        Tortoise ORM is designed to be functional, yet familiar, to ease the migration of developers wishing to switch to ``asyncio``.

        It also performs well when compared to other Python ORMs, trading places with Pony ORM:

        .. image:: ORM_Perf.png
            :target: https://github.com/tortoise/orm-benchmarks

ORM 有何用处？
---------------------

**How is an ORM useful?**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        当您构建一个使用关系数据库的应用程序或服务时，有一个阶段，您无法仅依靠参数化查询或查询构建器，您会不断重复自己，为每个实体编写稍微不同的代码。
        代码对数据之间的关系没有概念，因此您最终几乎是手动连接数据。
        在访问数据库时也很容易犯错，这使得 SQL 注入攻击的发生变得容易。
        您的数据规则也是分散的，增加了管理数据的复杂性，更糟糕的是，这些规则可能不一致地应用。

        ORM（对象关系映射器）旨在解决这些问题，通过集中您的数据模型和数据规则，确保安全地管理您的数据（提供对 SQL 注入的免疫），并跟踪关系，以便您无需手动处理。

    .. md-tab-item:: 英文

        **How is an ORM useful?**

        When you build an application or service that uses a relational database, there is a point when you can't just get away with just using parametrized queries or even query builder, you just keep repeating yourself, writing slightly different code for each entity.
        Code has no idea about relations between data, so you end up concatenating your data almost manually.
        It is also easy to make a mistake in how you access your database, making it easy for SQL-injection attacks to occur.
        Your data rules are also distributed, increasing the complexity of managing your data, and even worse, is applied inconsistently.

        An ORM (Object Relational Mapper) is designed to address these issues, by centralizing your data model and data rules, ensuring that your data is managed safely (providing immunity to SQL-injection) and keeps track of relationships so you don't have to.

功能
========

**Features**

干净、熟悉的 Python 界面
--------------------------------

**Clean, familiar python interface**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        定义您的模型如下：

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()

        初始化您的模型和数据库如下：

        .. code-block:: python3

            from tortoise import Tortoise, run_async

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

            # run_async 是一个帮助函数，用于运行简单的异步 Tortoise 脚本。
            run_async(init())

        并像这样使用它：

        .. code-block:: python3

            # 通过保存创建实例
            tournament = Tournament(name='新锦标赛')
            await tournament.save()

            # 或者通过 .create()
            await Tournament.create(name='另一个锦标赛')

            # 现在搜索记录
            tour = await Tournament.filter(name__contains='另一个').first()
            print(tour.name)
        
    .. md-tab-item:: 英文

        Define your models like so:

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()

        Initialise your models and database like so:

        .. code-block:: python3

            from tortoise import Tortoise, run_async

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

            # run_async is a helper function to run simple async Tortoise scripts.
            run_async(init())

        And use it like so:

        .. code-block:: python3

            # Create instance by save
            tournament = Tournament(name='New Tournament')
            await tournament.save()

            # Or by .create()
            await Tournament.create(name='Another Tournament')

            # Now search for a record
            tour = await Tournament.filter(name__contains='Another').first()
            print(tour.name)


可插入数据库后端
---------------------------

**Pluggable Database backends**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 目前支持以下 :ref:`databases`:

        * `PostgreSQL` >= 9.4（使用 ``asyncpg``）
        * `SQLite` （使用 ``aiosqlite``）
        * `MySQL`/`MariaDB` （使用 `asyncmy <https://github.com/long2ice/asyncmy>`_）
        * `Microsoft SQL Server`/`Oracle` （使用 ``asyncodbc``）
        
    .. md-tab-item:: 英文

        Tortoise ORM currently supports the following :ref:`databases`:

        * `PostgreSQL` >= 9.4 (using ``asyncpg``)
        * `SQLite` (using ``aiosqlite``)
        * `MySQL`/`MariaDB` (using `asyncmy <https://github.com/long2ice/asyncmy>`_)
        * `Microsoft SQL Server`/`Oracle` (using ``asyncodbc``)

以及更多
--------

**And more**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 支持以下功能：

        * 设计为可用于现有项目：
            * 测试框架使用现有的 Python Unittest 框架，只需要调用 ``initializer()`` 和 ``finalizer()`` 来设置和拆除测试数据库。（请参见 :ref:`unittest`）
            * ORM :ref:`init_app` 完全从提供的参数配置
        * 可组合的，受 Django 启发的 :ref:`models`
        * 支持关系，例如 ``ForeignKeyField`` 和 ``ManyToManyField``
        * 支持许多标准 :ref:`fields`
        * 综合 :ref:`query_api`
        * 事务 :ref:`transactions`
        * :ref:`pylint`

        如果您想贡献，请查看问题，或者直接创建 PR。
        
    .. md-tab-item:: 英文

        Tortoise ORM supports the following features:

        * Designed to be used in an existing project:
            * Testing framework uses existing Python Unittest framework, just requires
            that ``initializer()`` and ``finalizer()`` gets called to set up and tear
            down the test databases. (See :ref:`unittest`)
            * ORM :ref:`init_app` configures entirely from provided parameters
        * Composable, Django-inspired :ref:`models`
        * Supports relations, such as ``ForeignKeyField`` and ``ManyToManyField``
        * Supports many standard :ref:`fields`
        * Comprehensive :ref:`query_api`
        * Transactions :ref:`transactions`
        * :ref:`pylint`

        If you want to contribute check out issues, or just straightforwardly create PR
