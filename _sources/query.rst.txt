:tocdepth: 3

.. _query_api:

=========
查询API
=========

**Query API**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        本文件描述了如何使用 QuerySet 构建查询。

        请确保查看 `examples <https://github.com/tortoise/tortoise-orm/tree/master/examples>`_ 以便更好地理解。

        你可以从模型类开始你的查询：

        .. code-block:: python3

            Event.filter(id=1)

        模型本身有几个方法来开始查询：

        - ``filter(*args, **kwargs)`` - 创建带有给定过滤条件的 QuerySet
        - ``exclude(*args, **kwargs)`` - 创建带有给定排除条件的 QuerySet
        - ``all()`` - 创建不带过滤条件的 QuerySet
        - ``first()`` - 创建限于一个对象的 QuerySet，并返回实例而不是列表
        - ``annotate()`` - 创建带有给定注释的 QuerySet

        这些方法返回一个 ``QuerySet`` 对象，允许进一步的过滤和一些更复杂的操作。

        模型类还具有以下方法来创建对象：

        - ``create(**kwargs)`` - 使用给定的 kwargs 创建对象
        - ``get_or_create(defaults, **kwargs)`` - 获取给定 kwargs 的对象，如果未找到，则使用来自 defaults 字典的附加 kwargs 创建它

        模型实例本身还具有这些方法：

        - ``save()`` - 更新实例，或在从未保存之前插入它
        - ``delete()`` - 从数据库中删除实例
        - ``fetch_related(*args)`` - 获取与实例相关的对象。它可以获取外键关系、反向外键关系和多对多关系。它还可以获取可变深度的相关对象，例如：``await team.fetch_related('events__tournament')`` - 这将获取团队的所有事件，并且每个事件的锦标赛也将被预取。在获取对象后，它们应该可以正常使用，例如：``team.events[0].tournament.name``

        与实例上相关对象的另一种处理方法是在 ``async for`` 中显式查询它们：

        .. code-block:: python3

            async for team in event.participants:
                print(team.name)

        你也可以这样过滤相关对象：

        .. code-block:: python3

            await team.events.filter(name='First')

        这将返回一个带有预定义过滤条件的 QuerySet 对象。

    .. md-tab-item:: 英文

        This document describes how to use QuerySet to build your queries

        Be sure to check `examples <https://github.com/tortoise/tortoise-orm/tree/master/examples>`_ for better understanding

        You start your query from your model class:

        .. code-block:: python3

            Event.filter(id=1)

        There are several method on model itself to start query:

        - ``filter(*args, **kwargs)`` - create QuerySet with given filters
        - ``exclude(*args, **kwargs)`` - create QuerySet with given excluding filters
        - ``all()`` - create QuerySet without filters
        - ``first()`` - create QuerySet limited to one object and returning instance instead of list
        - ``annotate()`` - create QuerySet with given annotation

        This method returns ``QuerySet`` object, that allows further filtering and some more complex operations

        Also model class have this methods to create object:

        - ``create(**kwargs)`` - creates object with given kwargs
        - ``get_or_create(defaults, **kwargs)`` - gets object for given kwargs, if not found create it with additional kwargs from defaults dict

        Also instance of model itself has these methods:

        - ``save()`` - update instance, or insert it, if it was never saved before
        - ``delete()`` - delete instance from db
        - ``fetch_related(*args)`` - fetches objects related to instance. It can fetch FK relation, Backward-FK relations and M2M relations. It also can fetch variable depth of related objects like this: ``await team.fetch_related('events__tournament')`` - this will fetch all events for team, and for each of this events their tournament will be prefetched too. After fetching objects they should be available normally like this: ``team.events[0].tournament.name``

        Another approach to work with related objects on instance is to query them explicitly in ``async for``:

        .. code-block:: python3

            async for team in event.participants:
                print(team.name)

        You also can filter related objects like this:

        .. code-block:: python3

            await team.events.filter(name='First')

        which will return you a QuerySet object with predefined filter

QuerySet
========

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在你从对象获得查询集后，可以对其执行以下操作：

        .. automodule:: tortoise.queryset
            :members:
            :exclude-members: QuerySetSingle, QuerySet, AwaitableQuery

            .. autoclass:: QuerySetSingle

            .. autoclass:: QuerySet
                :inherited-members:

        QuerySet 可以构建、过滤并在不实际访问数据库的情况下传递。只有在你 ``await`` 查询集后，它才会生成查询并在数据库中运行。

        以下是一些使用查询集的常见场景（我们使用在 :ref:`getting_started` 中定义的模型）：

        常规选择模型实例：

        .. code-block:: python3

            await Event.filter(name__startswith='FIFA')

        该查询将获取所有 ``name`` 以 ``FIFA`` 开头的事件，其中 ``name`` 是模型中定义的字段，``startswith`` 是过滤修饰符。请注意，修饰符应通过双下划线分隔。你可以在本文档的 ``Filtering`` 部分阅读有关过滤修饰符的更多信息。

        你也可以使用 ``.exclude()`` 来过滤查询：

        .. code-block:: python3

            await Team.exclude(name__icontains='junior')

        作为更有趣的案例，当你处理相关数据时，你还可以围绕相关实体构建查询：

        .. code-block:: python3

            # 获取所有锦标赛名称为 "World Cup" 的事件
            await Event.filter(tournament__name='World Cup')

            # 获取参与事件 ID 为 1、2、3 的所有团队
            await Team.filter(events__id__in=[1, 2, 3])

            # 获取参与锦标赛的名称中包含 "junior" 的团队的所有锦标赛
            await Tournament.filter(event__participants__name__icontains='junior').distinct()

        通常，你不仅想按相关数据进行过滤，还希望获取该相关数据。你可以使用 ``.prefetch_related()`` 来做到这一点：

        .. code-block:: python3

            # 这将获取事件，并且每个事件的 ``.tournament`` 字段将填充相应的 ``Tournament`` 实例
            await Event.all().prefetch_related('tournament')

            # 这将获取锦标赛及其事件和每个事件的团队
            tournament_list = await Tournament.all().prefetch_related('events__participants')

            # 获取的多对多和反向外键关系的结果存储在类似列表的容器中
            for tournament in tournament_list:
                print([e.name for e in tournament.events])

        关于 ``prefetch_related()`` 的一般规则是，每个相关模型的深度层级产生一个额外的查询，因此 ``.prefetch_related('events__participants')`` 将产生两个额外的查询来获取你的数据。

        有时，当性能至关重要时，你不希望进行额外的查询。在这种情况下，你可以使用 ``values()`` 或 ``values_list()`` 来生成更高效的查询。

        .. code-block:: python3

            # 这将返回包含键 'id'、'name' 和 'tournament_name' 的字典列表，
            # 'tournament_name' 将由相关锦标赛的名称填充。
            # 并且将通过一个查询完成
            events = await Event.filter(id__in=[1, 2, 3]).values('id', 'name', tournament_name='tournament__name')

        查询集还通过 ``.annotate()`` 方法支持聚合和数据库函数。

        .. code-block:: python3

            from tortoise.functions import Count, Trim, Lower, Upper, Coalesce

            # 该查询将获取所有事件数量为 10 或更多的锦标赛，并将
            # 在实例中填充字段 `.events_count`，其值相应
            await Tournament.annotate(events_count=Count('events')).filter(events_count__gte=10)
            await Tournament.annotate(clean_name=Trim('name')).filter(clean_name='tournament')
            await Tournament.annotate(name_upper=Upper('name')).filter(name_upper='TOURNAMENT')
            await Tournament.annotate(name_lower=Lower('name')).filter(name_lower='tournament')
            await Tournament.annotate(desc_clean=Coalesce('desc', '')).filter(desc_clean='')

        请查看 `examples <https://github.com/tortoise/tortoise-orm/tree/master/examples>`_ 以了解其工作原理。

    .. md-tab-item:: 英文

        After you obtained queryset from object you can do following operations with it:

        .. automodule:: tortoise.queryset
            :members:
            :exclude-members: QuerySetSingle, QuerySet, AwaitableQuery

            .. autoclass:: QuerySetSingle

            .. autoclass:: QuerySet
                :inherited-members:

        QuerySet could be constructed, filtered and passed around without actually hitting database.
        Only after you ``await`` QuerySet, it will generate query and run it against database.

        Here are some common usage scenarios with QuerySet (we are using models defined in :ref:`getting_started`):

        Regular select into model instances:

        .. code-block:: python3

            await Event.filter(name__startswith='FIFA')

        This query will get you all events with ``name`` starting with ``FIFA``, where ``name`` is fields
        defined on model, and ``startswith`` is filter modifier. Take note, that modifiers should
        be separated by double underscore. You can read more on filter modifiers in ``Filtering``
        section of this document.

        It's also possible to filter your queries with ``.exclude()``:

        .. code-block:: python3

            await Team.exclude(name__icontains='junior')

        As more interesting case, when you are working with related data, you could also build your
        query around related entities:

        .. code-block:: python3

            # getting all events, which tournament name is "World Cup"
            await Event.filter(tournament__name='World Cup')

            # Gets all teams participating in events with ids 1, 2, 3
            await Team.filter(events__id__in=[1,2,3])

            # Gets all tournaments where teams with "junior" in their name are participating
            await Tournament.filter(event__participants__name__icontains='junior').distinct()


        Usually you not only want to filter by related data, but also get that related data as well.
        You could do it using ``.prefetch_related()``:

        .. code-block:: python3

            # This will fetch events, and for each of events ``.tournament`` field will be populated with
            # corresponding ``Tournament`` instance
            await Event.all().prefetch_related('tournament')

            # This will fetch tournament with their events and teams for each event
            tournament_list = await Tournament.all().prefetch_related('events__participants')

            # Fetched result for m2m and backward fk relations are stored in list-like container
            for tournament in tournament_list:
                print([e.name for e in tournament.events])


        General rule about how ``prefetch_related()`` works is that each level of depth of related models
        produces 1 additional query, so ``.prefetch_related('events__participants')`` will produce two
        additional queries to fetch your data.

        Sometimes, when performance is crucial, you don't want to make additional queries like this.
        In cases like this you could use ``values()`` or ``values_list()`` to produce more efficient query

        .. code-block:: python3

            # This will return list of dicts with keys 'id', 'name', 'tournament_name' and
            # 'tournament_name' will be populated by name of related tournament.
            # And it will be done in one query
            events = await Event.filter(id__in=[1,2,3]).values('id', 'name', tournament_name='tournament__name')

        QuerySet also supports aggregation and database functions through ``.annotate()`` method

        .. code-block:: python3

            from tortoise.functions import Count, Trim, Lower, Upper, Coalesce

            # This query will fetch all tournaments with 10 or more events, and will
            # populate filed `.events_count` on instances with corresponding value
            await Tournament.annotate(events_count=Count('events')).filter(events_count__gte=10)
            await Tournament.annotate(clean_name=Trim('name')).filter(clean_name='tournament')
            await Tournament.annotate(name_upper=Upper('name')).filter(name_upper='TOURNAMENT')
            await Tournament.annotate(name_lower=Lower('name')).filter(name_lower='tournament')
            await Tournament.annotate(desc_clean=Coalesce('desc', '')).filter(desc_clean='')

        Check `examples <https://github.com/tortoise/tortoise-orm/tree/master/examples>`_ to see it all in work

.. _foreign_key:

外键
===========

**Foreign Key**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 提供了用于处理 FK 关系的 API
    
    .. md-tab-item:: 英文

        Tortoise ORM provides an API for working with FK relations

.. autoclass:: tortoise.fields.relational.ReverseRelation
    :members:

.. autodata:: tortoise.fields.relational.ForeignKeyNullableRelation

.. autodata:: tortoise.fields.relational.ForeignKeyRelation

.. _one_to_one:

一对一
==========

**One to One**

.. autodata:: tortoise.fields.relational.OneToOneNullableRelation

.. autodata:: tortoise.fields.relational.OneToOneRelation

.. _many_to_many:

多对多
============

**Many to Many**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 提供了一个用于处理多对多（M2M）关系的 API。

        .. autoclass:: tortoise.fields.relational.ManyToManyRelation
            :members:
            :inherited-members:

        你可以这样使用它们：

        .. code-block:: python3

            await event.participants.add(participant_1, participant_2)

    .. md-tab-item:: 英文

        Tortoise ORM provides an API for working with M2M relations

        .. autoclass:: tortoise.fields.relational.ManyToManyRelation
            :members:
            :inherited-members:

        You can use them like this:

        .. code-block:: python3

            await event.participants.add(participant_1, participant_2)


.. _filtering-queries:

过滤
=========

Filtering

.. md-tab-set::
    
    .. md-tab-item:: 中文

        使用 ``.filter()`` 方法时，可以使用多个修饰符来指定所需的操作。

        .. code-block:: python3

            teams = await Team.filter(name__icontains='CON')

        - ``not``
        - ``in`` - 检查字段的值是否在传入的列表中
        - ``not_in``
        - ``gte`` - 大于或等于传入的值
        - ``gt`` - 大于传入的值
        - ``lte`` - 小于或等于传入的值
        - ``lt`` - 小于传入的值
        - ``range`` - 在两个给定值之间
        - ``isnull`` - 字段为 null
        - ``not_isnull`` - 字段不为 null
        - ``contains`` - 字段包含指定子字符串
        - ``icontains`` - 不区分大小写的 ``contains``
        - ``startswith`` - 字段是否以值开头
        - ``istartswith`` - 不区分大小写的 ``startswith``
        - ``endswith`` - 字段是否以值结尾
        - ``iendswith`` - 不区分大小写的 ``endswith``
        - ``iexact`` - 不区分大小写的相等
        - ``search`` - 全文搜索

        特别地，你可以使用以下之一过滤日期部分，注意目前只支持 PostgreSQL 和 MySQL，但不支持 SQLite：

        .. code:: python3

            class DatePart(Enum):
                year = "YEAR"
                quarter = "QUARTER"
                month = "MONTH"
                week = "WEEK"
                day = "DAY"
                hour = "HOUR"
                minute = "MINUTE"
                second = "SECOND"
                microsecond = "MICROSECOND"

            teams = await Team.filter(created_at__year=2020)
            teams = await Team.filter(created_at__month=12)
            teams = await Team.filter(created_at__day=5)

        在 PostgreSQL 和 MySQL 中，你可以在 ``JSONField`` 中使用 ``contains``、``contained_by`` 和 ``filter`` 选项：

        .. code:: python3

            class JSONModel:
                data = fields.JSONField()

            await JSONModel.create(data=["text", 3, {"msg": "msg2"}])
            obj = await JSONModel.filter(data__contains=[{"msg": "msg2"}]).first()

            await JSONModel.create(data=["text"])
            await JSONModel.create(data=["tortoise", "msg"])
            await JSONModel.create(data=["tortoise"])

            objects = await JSONModel.filter(data__contained_by=["text", "tortoise", "msg"])

        .. code-block:: python3

            class JSONModel:
                data = fields.JSONField()

            await JSONModel.create(data={"breed": "labrador",
                                        "owner": {
                                            "name": "Boby",
                                            "last": None,
                                            "other_pets": [
                                                {
                                                    "name": "Fishy",
                                                }
                                            ],
                                        },
                                    })

            obj1 = await JSONModel.filter(data__filter={"breed": "labrador"}).first()
            obj2 = await JSONModel.filter(data__filter={"owner__name": "Boby"}).first()
            obj3 = await JSONModel.filter(data__filter={"owner__other_pets__0__name": "Fishy"}).first()
            obj4 = await JSONModel.filter(data__filter={"breed__not": "a"}).first()
            obj5 = await JSONModel.filter(data__filter={"owner__name__isnull": True}).first()
            obj6 = await JSONModel.filter(data__filter={"owner__last__not_isnull": False}).first()

        在 PostgreSQL 和 MySQL 中，你可以使用 ``postgres_posix_regex`` 使用 POSIX 正则表达式进行比较：
        在 PostgreSQL 中，这使用 ``~`` 操作符，在 MySQL 中它使用 ``REGEXP`` 操作符。

        .. code-block:: python3

            class DemoModel:
            
                demo_text = fields.TextField()

            await DemoModel.create(demo_text="Hello World")
            obj = await DemoModel.filter(demo_text__posix_regex="^Hello World$").first()

    .. md-tab-item:: 英文

        When using ``.filter()`` method you can use number of modifiers to field names to specify desired operation

        .. code-block:: python3

            teams = await Team.filter(name__icontains='CON')

        - ``not``
        - ``in`` - checks if value of field is in passed list
        - ``not_in``
        - ``gte`` - greater or equals than passed value
        - ``gt`` - greater than passed value
        - ``lte`` - lower or equals than passed value
        - ``lt`` - lower than passed value
        - ``range`` - between and given two values
        - ``isnull`` - field is null
        - ``not_isnull`` - field is not null
        - ``contains`` - field contains specified substring
        - ``icontains`` - case insensitive ``contains``
        - ``startswith`` - if field starts with value
        - ``istartswith`` - case insensitive ``startswith``
        - ``endswith`` - if field ends with value
        - ``iendswith`` - case insensitive ``endswith``
        - ``iexact`` - case insensitive equals
        - ``search`` - full text search

        Specially, you can filter date part with one of following, note that current only support PostgreSQL and MySQL, but not sqlite:

        .. code:: python3

            class DatePart(Enum):
                year = "YEAR"
                quarter = "QUARTER"
                month = "MONTH"
                week = "WEEK"
                day = "DAY"
                hour = "HOUR"
                minute = "MINUTE"
                second = "SECOND"
                microsecond = "MICROSECOND"

            teams = await Team.filter(created_at__year=2020)
            teams = await Team.filter(created_at__month=12)
            teams = await Team.filter(created_at__day=5)

        In PostgreSQL and MYSQL, you can use the ``contains``, ``contained_by`` and ``filter`` options in ``JSONField``:

        .. code:: python3

            class JSONModel:
                data = fields.JSONField()

            await JSONModel.create(data=["text", 3, {"msg": "msg2"}])
            obj = await JSONModel.filter(data__contains=[{"msg": "msg2"}]).first()

            await JSONModel.create(data=["text"])
            await JSONModel.create(data=["tortoise", "msg"])
            await JSONModel.create(data=["tortoise"])

            objects = await JSONModel.filter(data__contained_by=["text", "tortoise", "msg"])

        .. code-block:: python3

            class JSONModel:
                data = fields.JSONField()

            await JSONModel.create(data={"breed": "labrador",
                                        "owner": {
                                            "name": "Boby",
                                            "last": None,
                                            "other_pets": [
                                                {
                                                    "name": "Fishy",
                                                }
                                            ],
                                        },
                                    })

            obj1 = await JSONModel.filter(data__filter={"breed": "labrador"}).first()
            obj2 = await JSONModel.filter(data__filter={"owner__name": "Boby"}).first()
            obj3 = await JSONModel.filter(data__filter={"owner__other_pets__0__name": "Fishy"}).first()
            obj4 = await JSONModel.filter(data__filter={"breed__not": "a"}).first()
            obj5 = await JSONModel.filter(data__filter={"owner__name__isnull": True}).first()
            obj6 = await JSONModel.filter(data__filter={"owner__last__not_isnull": False}).first()

        In PostgreSQL and MySQL, you can use ``postgres_posix_regex`` to make comparisons using POSIX regular expressions:
        On PostgreSQL, this uses the ``~`` operator, on MySQL it uses the ``REGEXP`` operator.

        .. code-block:: python3

            class DemoModel:
            
                demo_text = fields.TextField()

            await DemoModel.create(demo_text="Hello World")
            obj = await DemoModel.filter(demo_text__posix_regex="^Hello World$").first()


复杂的预获取
================

**Complex prefetch**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        有时需要仅提取某些相关记录。你可以使用 ``Prefetch`` 对象来实现：

        .. code-block:: python3

            tournament_with_filtered = await Tournament.all().prefetch_related(
                Prefetch('events', queryset=Event.filter(name='First'))
            ).first()

        你可以在这里查看完整示例： :ref:`example_prefetching`

    .. md-tab-item:: 英文

        Sometimes it is required to fetch only certain related records. You can achieve it with ``Prefetch`` object:

        .. code-block:: python3

            tournament_with_filtered = await Tournament.all().prefetch_related(
                Prefetch('events', queryset=Event.filter(name='First'))
            ).first()

        You can view full example here:  :ref:`example_prefetching`

.. autoclass:: tortoise.query_utils.Prefetch
    :members:
