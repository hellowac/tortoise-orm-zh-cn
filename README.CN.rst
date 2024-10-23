============
Tortoise ORM
============

.. image:: https://img.shields.io/pypi/v/tortoise-orm.svg?style=flat
   :target: https://pypi.python.org/pypi/tortoise-orm
.. image:: https://pepy.tech/badge/tortoise-orm/month
   :target: https://pepy.tech/project/tortoise-orm
.. image:: https://github.com/tortoise/tortoise-orm/workflows/gh-pages/badge.svg
   :target: https://github.com/tortoise/tortoise-orm/actions?query=workflow:gh-pages
.. image:: https://github.com/tortoise/tortoise-orm/actions/workflows/ci.yml/badge.svg?branch=develop
   :target: https://github.com/tortoise/tortoise-orm/actions?query=workflow:ci
.. image:: https://coveralls.io/repos/github/tortoise/tortoise-orm/badge.svg
   :target: https://coveralls.io/github/tortoise/tortoise-orm
.. image:: https://app.codacy.com/project/badge/Grade/844030d0cb8240d6af92c71bfac764ff
   :target: https://www.codacy.com/gh/tortoise/tortoise-orm/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=tortoise/tortoise-orm&amp;utm_campaign=Badge_Grade

引言(Introduction)
========================

Tortoise ORM 是一个易于使用的 ``asyncio`` ORM *(对象关系映射器)*，受 Django 启发。

Tortoise ORM 的设计考虑了关系，并对优秀且流行的 Django ORM 表示钦佩。
它的设计中铭刻着您不仅在处理表格，而是在处理关系数据。

您可以在 `文档 <https://tortoise.github.io>`_ 中找到相关资料。

.. note::
   Tortoise ORM 是一个年轻的项目，预计会有重大变更。
   我们维护一个 `变更日志 <https://tortoise.github.io/CHANGELOG.html>`_，可能的破坏性更改会被清晰记录。

Tortoise ORM 支持 CPython >= 3.8，适用于 SQLite、MySQL、PostgreSQL、Microsoft SQL Server 和 Oracle。

为什么要构建 Tortoise ORM？(Why was Tortoise ORM built?)
------------------------------------------------------

Python 已有许多现存且成熟的 ORM，不幸的是，它们的设计与 I/O 处理的对立范式相悖。
``asyncio`` 是一种相对较新的技术，具有非常不同的并发模型，最大的变化在于 I/O 的处理方式。

然而，Tortoise ORM 并不是构建 ``asyncio`` ORM 的第一次尝试。虽然许多开发者试图将同步 Python ORM 映射到异步世界，但最初的尝试并没有一个干净的 API。

因此，我们开始了 Tortoise ORM。

Tortoise ORM 的设计旨在功能性与熟悉性兼备，以便于希望切换到 ``asyncio`` 的开发者的迁移。

与其他 Python ORM 相比，它的性能也表现良好。在我们的基准测试中 <https://github.com/tortoise/orm-benchmarks>`_，我们测量了不同的读写操作（行/秒，越多越好），它与 Pony ORM 交替领先：

.. image:: https://raw.githubusercontent.com/tortoise/tortoise-orm/develop/docs/ORM_Perf.png
    :target: https://github.com/tortoise/orm-benchmarks

ORM 如何有用？(How is an ORM useful?)
---------------------

当您构建一个使用关系数据库的应用程序或服务时，您会发现单靠参数化查询或查询构建器是无法满足需求的。您会不断重复自己，为每个实体编写稍微不同的代码。
代码对数据之间的关系没有概念，因此您最终几乎是手动连接数据。
在访问数据库时也很容易犯错误，这可能会被 SQL 注入攻击所利用。
您的数据规则分散，增加了管理数据的复杂性，更糟糕的是，可能导致这些规则不一致地应用。

ORM（对象关系映射器）旨在解决这些问题，通过集中您的数据模型和数据规则，确保安全地管理您的数据（提供对 SQL 注入的免疫），并跟踪关系，以便您无需手动处理。


开始使用(Getting Started)
==============================

安装(Installation)
------------------------

首先，您需要像这样安装 Tortoise ORM：

.. code-block:: bash

    pip install tortoise-orm

您也可以与您的数据库驱动一起安装（`aiosqlite` 是内置的）：

.. code-block:: bash

    pip install "tortoise-orm[asyncpg]"

对于 `MySQL`：

.. code-block:: bash

    pip install "tortoise-orm[asyncmy]"

对于 `Microsoft SQL Server`/`Oracle`（**未完全测试**）：

.. code-block:: bash

    pip install "tortoise-orm[asyncodbc]"

快速教程(Quick Tutorial)
----------------------------

Tortoise 的主要实体是 ``tortoise.models.Model``。
您可以像这样开始编写模型：

.. code-block:: python3

    from tortoise.models import Model
    from tortoise import fields

    class Tournament(Model):
        id = fields.IntField(primary_key=True)
        name = fields.TextField()

        def __str__(self):
            return self.name


    class Event(Model):
        id = fields.IntField(primary_key=True)
        name = fields.TextField()
        tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
        participants = fields.ManyToManyField('models.Team', related_name='events', through='event_team')

        def __str__(self):
            return self.name


    class Team(Model):
        id = fields.IntField(primary_key=True)
        name = fields.TextField()

        def __str__(self):
            return self.name


在定义完所有模型后，Tortoise 需要您初始化它们，以便在模型之间创建反向关系，并将您的数据库客户端与相应的模型匹配。

您可以这样做：

.. code-block:: python3

    from tortoise import Tortoise

    async def init():
        # 这里我们连接到一个 SQLite 数据库文件。
        # 同时指定应用名称为 "models"
        # 其中包含来自 "app.models" 的模型
        await Tortoise.init(
            db_url='sqlite://db.sqlite3',
            modules={'models': ['app.models']}
        )
        # 生成模式
        await Tortoise.generate_schemas()


在这里，我们创建了一个连接到本地目录中名为 ``db.sqlite3`` 的 SQLite 数据库。然后我们发现并初始化模型。

Tortoise ORM 目前支持以下数据库：

* `SQLite`（需要 ``aiosqlite``）
* `PostgreSQL`（需要 ``asyncpg``）
* `MySQL`（需要 ``asyncmy``）
* `Microsoft SQL Server`/`Oracle`（需要 ``asyncodbc``）

``generate_schema`` 在空数据库上生成模式。Tortoise 默认以安全模式生成模式，包含 ``IF NOT EXISTS`` 子句，因此您可以在主代码中包含它。

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

    # M2M 关系管理相当简单
    # （还可以查看方法 .remove(...) 和 .clear()）
    await event.participants.add(*participants)

    # 您可以使用 async for 查询相关实体
    async for team in event.participants:
        pass

    # 在进行相关查询后，您可以使用常规 for 进行迭代，
    # 这在与其他包一起使用时非常方便，
    # 例如某种支持嵌套的序列化器
    for team in event.participants:
        pass


    # 或者您可以提前调用以获取相关对象
    selected_events = await Event.filter(
        participants=participants[0].id
    ).prefetch_related('participants', 'tournament')

    # Tortoise 支持可变深度的预取相关实体
    # 这将获取团队的所有事件，并在这些事件中预取锦标赛
    await Team.all().prefetch_related('events__tournament')

    # 您也可以根据相关模型进行过滤和排序
    await Tournament.filter(
        events__name__in=['测试', '生产']
    ).order_by('-events__participants__name').distinct()

迁移(Migration)
==================

Tortoise ORM 使用 `Aerich <https://github.com/tortoise/aerich>`_ 作为其数据库迁移工具，详细信息请参见其 `文档 <https://github.com/tortoise/aerich>`_。

贡献(Contributing)
========================

请查看 `贡献指南 <docs/CONTRIBUTING.rst>`_。

感谢(ThanksTo)
================

强大的 Python IDE `Pycharm <https://www.jetbrains.com/pycharm/>`_
来自 `Jetbrains <https://jb.gg/OpenSourceSupport>`_。

.. image:: https://resources.jetbrains.com/storage/products/company/brand/logos/jb_beam.svg
    :target: https://jb.gg/OpenSourceSupport

许可证(License)
=====================

本项目根据 Apache 许可证发布 - 详细信息请参见 `LICENSE.txt <LICENSE.txt>`_ 文件。
