.. _getting_started:

===============
开始入门
===============

**Getting started**

安装
===============

**Installation**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        首先，您必须像这样安装 tortoise：

        .. code-block:: bash

            pip install tortoise-orm

        ..

        您也可以使用您的 db 驱动程序进行安装：

        .. code-block:: bash

            pip install tortoise-orm[asyncpg]

        ..

        或者 PsycoPG：

        .. code-block:: bash

            pip install tortoise-orm[psycopg]

        ..

        对于 MySQL：

        .. code-block:: bash

            pip install tortoise-orm[asyncmy]

        ..

        对于 Microsoft SQL Server/Oracle：

        .. code-block:: bash

            pip install tortoise-orm[asyncodbc]

        ..

        除了 ``asyncpg`` 和 ``psycopg``，还通过 ``aiosqlite`` 支持 ``sqlite``，通过 ``asyncmy`` 支持 ``mysql``。
        如果有适当的 ``asyncio`` 驱动程序用于此数据库，您可以轻松实现更多后端。
        
    .. md-tab-item:: 英文

        First you have to install tortoise like this:

        .. code-block:: bash

            pip install tortoise-orm

        ..

        You can also install with your db driver:

        .. code-block:: bash

            pip install tortoise-orm[asyncpg]

        ..

        Or PsycoPG:

        .. code-block:: bash

            pip install tortoise-orm[psycopg]

        ..

        For MySQL:

        .. code-block:: bash

            pip install tortoise-orm[asyncmy]

        ..

        For Microsoft SQL Server/Oracle:

        .. code-block:: bash

            pip install tortoise-orm[asyncodbc]

        ..

        Apart from ``asyncpg`` and ``psycopg`` there is also support for ``sqlite`` through ``aiosqlite`` and
        ``mysql`` through ``asyncmy``.
        You can easily implement more backends if there is appropriate ``asyncio`` driver for this db.

可选加速器
---------------------

**Optional Accelerators**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        以下库可以用作加速器：

        * `orjson <https://pypi.org/project/orjson/>`_: 如果安装，将自动用于 JSON 序列化和反序列化。
        * `uvloop <https://pypi.org/project/uvloop/>`_: 被证明可以提高性能，但需要设置。有关更多信息，请查看 ``uvloop`` 文档。如果您使用框架，它可能已经使用了它。
        * `ciso8601 <https://pypi.org/project/ciso8601/>`_: 如果安装，将自动使用。由于常常缺少 C 编译器，在 Windows 上不会自动安装。在 Linux/CPython 上是默认的。

        您可以安装上述所有加速器：

        .. code-block:: bash

            pip install tortoise-orm[accel]

        ..
        
    .. md-tab-item:: 英文

        The following libraries can be used as accelerators:

        * `orjson <https://pypi.org/project/orjson/>`_: Automatically used if installed for JSON SerDes.
        * `uvloop <https://pypi.org/project/uvloop/>`_: Shown to improve performance, but needs to be set up.
        Please look at ``uvloop`` documentation for more info.
        If you use a framework, it may already use it.
        * `ciso8601 <https://pypi.org/project/ciso8601/>`_: Automatically used if installed.
        Not automatically installed on Windows due to often a lack of a C compiler. Default on Linux/CPython.

        You can install with all accelerators above:

        .. code-block:: bash

            pip install tortoise-orm[accel]

        ..

教程
========

**Tutorial**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise 的主要实体是 ``tortoise.models.Model`` 。
        您可以像这样开始编写模型：

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                # 定义 `id` 字段是可选的，如果您没有自己定义，它会自动定义
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                # 定义 ``__str__`` 也是可选的，但可以在调试器和解释器中提供
                # 模型的漂亮表示
                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)
                # 对其他模型的引用按格式定义
                # "{app_name}.{model_name}" - 其中 {app_name} 在 tortoise 配置中定义
                tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
                participants = fields.ManyToManyField('models.Team', related_name='events', through='event_team')

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                def __str__(self):
                    return self.name

        .. note::
        
            您可以在 :ref:`models` 中阅读更多关于定义模型的信息。

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


        在这里，我们创建一个使用默认 ``aiosqlite`` 客户端的 SQLite 数据库连接，然后发现并初始化模型。

        ``generate_schema`` 在空数据库上生成模式，您不应该在每次应用初始化时运行它，可能只需运行一次，最好在主代码之外。

        如果您在简单脚本中运行此代码，可以这样做：

        .. code-block:: python3

            run_async(init())

        ``run_async`` 是一个帮助函数，用于运行简单的异步 Tortoise 脚本。如果您将 Tortoise ORM 作为服务的一部分运行，请查看 :ref:`cleaningup`。

        之后，您可以开始使用您的模型：

        .. code-block:: python3

            # 通过保存创建实例
            tournament = Tournament(name='新锦标赛')
            await tournament.save()

            # 或者通过 .create()
            await Event.create(name='无参与者', tournament=tournament)
            event = await Event.create(name='测试', tournament=tournament)
            participants = []
            for i in range(2):
                team = await Team.create(name='队伍 {}'.format(i + 1))
                participants.append(team)

            # M2M 关系管理非常简单
            # （请查看方法 .remove(...) 和 .clear()）
            await event.participants.add(*participants)

            # 您可以使用 async for 查询相关实体
            async for team in event.participants:
                pass

            # 进行相关查询后，您可以使用常规 for 迭代，
            # 这在与其他包一起使用时可能非常方便，
            # 例如某种带有嵌套支持的序列化器
            for team in event.participants:
                pass


            # 或者您可以提前调用以获取相关对象，
            # 这样您可以立即处理相关对象
            selected_events = await Event.filter(
                participants=participants[0].id
            ).prefetch_related('participants', 'tournament')
            for event in selected_events:
                print(event.tournament.name)
                print([t.name for t in event.participants])

            # Tortoise ORM 支持可变深度的预取相关实体
            # 这将获取所有队伍的事件，并在这些事件中预取锦标赛
            await Team.all().prefetch_related('events__tournament')

            # 您也可以按相关模型过滤和排序
            await Tournament.filter(
                events__name__in=['测试', '生产']
            ).order_by('-events__participants__name').distinct()

        .. note::
            您可以在 :ref:`examples` 中阅读更多示例（包括事务、多个数据库和更复杂的查询）。

    .. md-tab-item:: 英文

        Primary entity of tortoise is ``tortoise.models.Model``.
        You can start writing models like this:


        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                # Defining `id` field is optional, it will be defined automatically
                # if you haven't done it yourself
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                # Defining ``__str__`` is also optional, but gives you pretty
                # represent of model in debugger and interpreter
                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)
                # References to other models are defined in format
                # "{app_name}.{model_name}" - where {app_name} is defined in tortoise config
                tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
                participants = fields.ManyToManyField('models.Team', related_name='events', through='event_team')

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                def __str__(self):
                    return self.name

        .. note::
        You can read more on defining models in :ref:`models`

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


        Here we create a connection to a SQLite DB database with the default ``aiosqlite`` client and then we discover & initialise models.

        ``generate_schema`` generates schema on empty database, you shouldn't run it on every app init, run it just once, maybe out of your main code.

        If you are running this in a simple script, you can do:

        .. code-block:: python3

            run_async(init())

        ``run_async`` is a helper function to run simple async Tortoise scripts. If you are running Tortoise ORM as part of a service, please have a look at :ref:`cleaningup`

        After that you can start using your models:

        .. code-block:: python3

            # Create instance by save
            tournament = Tournament(name='New Tournament')
            await tournament.save()

            # Or by .create()
            await Event.create(name='Without participants', tournament=tournament)
            event = await Event.create(name='Test', tournament=tournament)
            participants = []
            for i in range(2):
                team = await Team.create(name='Team {}'.format(i + 1))
                participants.append(team)

            # M2M Relationship management is quite straightforward
            # (look for methods .remove(...) and .clear())
            await event.participants.add(*participants)

            # You can query related entity just with async for
            async for team in event.participants:
                pass

            # After making related query you can iterate with regular for,
            # which can be extremely convenient for using with other packages,
            # for example some kind of serializers with nested support
            for team in event.participants:
                pass


            # Or you can make preemptive call to fetch related objects,
            # so you can work with related objects immediately
            selected_events = await Event.filter(
                participants=participants[0].id
            ).prefetch_related('participants', 'tournament')
            for event in selected_events:
                print(event.tournament.name)
                print([t.name for t in event.participants])

            # Tortoise ORM supports variable depth of prefetching related entities
            # This will fetch all events for team and in those team tournament will be prefetched
            await Team.all().prefetch_related('events__tournament')

            # You can filter and order by related models too
            await Tournament.filter(
                events__name__in=['Test', 'Prod']
            ).order_by('-events__participants__name').distinct()

        .. note::
            You can read more examples (including transactions, several databases and a little more complex querying) in
            :ref:`examples`
