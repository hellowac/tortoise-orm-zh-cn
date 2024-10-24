:tocdepth: 4

.. _models:

======
模型
======

**Models**

.. rst-class:: emphasize-children

使用方法
==========

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        要开始使用模型，首先应该导入它们：

        .. code-block:: python3

            from tortoise.models import Model

        有了这个，你可以开始描述自己的模型，如下所示：

        .. code-block:: python3

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                created = fields.DatetimeField(auto_now_add=True)

                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
                participants = fields.ManyToManyField('models.Team', related_name='events', through='event_team')
                modified = fields.DatetimeField(auto_now=True)
                prize = fields.DecimalField(max_digits=10, decimal_places=2, null=True)

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()

                def __str__(self):
                    return self.name

        让我们详细看看我们在这里完成了什么：

        .. code-block:: python3

            class Tournament(Model):

        每个模型都应该从基础模型派生。你也可以从自己的模型子类派生，并且可以像这样创建抽象模型：

        .. code-block:: python3

            class AbstractTournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                created = fields.DatetimeField(auto_now_add=True)

                class Meta:
                    abstract = True

                def __str__(self):
                    return self.name

        这些模型不会在架构生成中被创建，也不会与其他模型建立关系。

        进一步来看，我们有字段 ``fields.DatetimeField(auto_now=True)``。选项 ``auto_now`` 和 ``auto_now_add`` 的工作方式类似于 Django 的选项。
    
    .. md-tab-item:: 英文

        To get working with models, first you should import them

        .. code-block:: python3

            from tortoise.models import Model

        With that you can start describing your own models like that

        .. code-block:: python3

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                created = fields.DatetimeField(auto_now_add=True)

                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
                participants = fields.ManyToManyField('models.Team', related_name='events', through='event_team')
                modified = fields.DatetimeField(auto_now=True)
                prize = fields.DecimalField(max_digits=10, decimal_places=2, null=True)

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()

                def __str__(self):
                    return self.name

        Let see in details what we accomplished here:

        .. code-block:: python3

            class Tournament(Model):

        Every model should be derived from base model. You also can derive from your own model subclasses and you can make abstract models like this

        .. code-block:: python3

            class AbstractTournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.TextField()
                created = fields.DatetimeField(auto_now_add=True)

                class Meta:
                    abstract = True

                def __str__(self):
                    return self.name

        This models won't be created in schema generation and won't create relations to other models.


        Further we have field ``fields.DatetimeField(auto_now=True)``. Options ``auto_now`` and ``auto_now_add`` work like Django's options.

``__models__`` 的用法
---------------------

Use of ``__models__``

.. md-tab-set::
    
    .. md-tab-item:: 中文

        如果在加载模型的模块中定义了变量 ``__models__``，则 ``generate_schema`` 将使用该列表，而不是自动查找模型。
    
    .. md-tab-item:: 英文

        If you define the variable ``__models__`` in the module which you load your models from, ``generate_schema`` will use that list, rather than automatically finding models for you.

主键
------------

**Primary Keys**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在 Tortoise ORM 中，我们要求每个模型都有一个主键。

        该主键可以通过一个保留字段 ``pk`` 访问，该字段是被指定为主键的字段的别名。这个别名字段可以在过滤时作为字段名称使用，例如 ``.filter(pk=...)`` 等等。

        .. note::

            我们当前支持任何可索引字段类型的单一（非组合）主键，但只推荐以下字段类型：

        .. code-block:: python3

            IntField
            BigIntField
            CharField
            UUIDField

        必须通过将 ``primary_key`` 参数设置为 ``True`` 来定义主键。如果不定义主键，我们将为您创建一个类型为 ``IntField`` 名称为 ``id`` 的主键。

        .. note::
        如果在整数字段上使用此选项，则 ``generated`` 将设置为 ``True``，除非您显式传递 ``generated=False``。

        以下任何定义都是模型中有效的主键定义：

        .. code-block:: python3

            id = fields.IntField(primary_key=True)

            checksum = fields.CharField(primary_key=True)

            guid = fields.UUIDField(primary_key=True)
    
    .. md-tab-item:: 英文

        In Tortoise ORM we require that a model has a primary key.

        That primary key will be accessible through a reserved field ``pk`` which will be an alias of whichever field has been nominated as a primary key.
        That alias field can be used as a field name when doing filtering e.g. ``.filter(pk=...)`` etc…

        .. note::

            We currently support single (non-composite) primary keys of any indexable field type, but only these field types are recommended:

        .. code-block:: python3

            IntField
            BigIntField
            CharField
            UUIDField

        One must define a primary key by setting a ``primary_key`` parameter to ``True``.
        If you don't define a primary key, we will create a primary key of type ``IntField`` with name of ``id`` for you.

        .. note::
        If this is used on an Integer Field, ``generated`` will be set to ``True`` unless you explicitly pass ``generated=False`` as well.

        Any of these are valid primary key definitions in a Model:

        .. code-block:: python3

            id = fields.IntField(primary_key=True)

            checksum = fields.CharField(primary_key=True)

            guid = fields.UUIDField(primary_key=True)


继承
-----------

**Inheritance**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在 Tortoise ORM 中定义模型时，可以通过利用继承来节省大量重复工作。

        您可以在更通用的类中定义字段，这些字段会自动在派生类中可用。基类不限于模型类，任何类都可以使用。这样，您能够以自然且易于维护的方式定义模型。

        让我们来看一些示例。

        .. code-block:: python3

            from tortoise import fields
            from tortoise.models import Model

            class TimestampMixin():
                created_at = fields.DatetimeField(null=True, auto_now_add=True)
                modified_at = fields.DatetimeField(null=True, auto_now=True)

            class NameMixin():
                name = fields.CharField(40, unique=True)

            class MyAbstractBaseModel(Model):
                id = fields.IntField(primary_key=True)

                class Meta:
                    abstract = True

            class UserModel(TimestampMixin, MyAbstractBaseModel):
                # 重写 MyAbstractBaseModel 的 id 定义
                id = fields.UUIDField(primary_key=True)

                # 添加额外字段
                first_name = fields.CharField(20, null=True)

                class Meta:
                    table = "user"


            class RoleModel(TimestampMixin, NameMixin, MyAbstractBaseModel):

                class Meta:
                    table = "role"

        使用 ``Meta`` 类不是必需的，但这是一个好习惯，可以为您的表提供一个明确的名称。这样，您可以在不破坏架构的情况下更改模型名称。因此，以下定义是有效的。

        .. code-block:: python3

            class RoleModel(TimestampMixin, NameMixin, MyAbstractBaseModel):

    .. md-tab-item:: 英文

        When defining models in Tortoise ORM, you can save a lot of
        repetitive work by leveraging from inheritance.

        You can define fields in more generic classes and they are
        automatically available in derived classes. Base classes are
        not limited to Model classes. Any class will work. This way
        you are able to define your models in a natural and easy
        to maintain way.

        Let's have a look at some examples.

        .. code-block:: python3

            from tortoise import fields
            from tortoise.models import Model

            class TimestampMixin():
                created_at = fields.DatetimeField(null=True, auto_now_add=True)
                modified_at = fields.DatetimeField(null=True, auto_now=True)

            class NameMixin():
                name = fields.CharField(40, unique=True)

            class MyAbstractBaseModel(Model):
                id = fields.IntField(primary_key=True)

                class Meta:
                    abstract = True

            class UserModel(TimestampMixin, MyAbstractBaseModel):
                # Overriding the id definition
                # from MyAbstractBaseModel
                id = fields.UUIDField(primary_key=True)

                # Adding additional fields
                first_name = fields.CharField(20, null=True)

                class Meta:
                    table = "user"


            class RoleModel(TimestampMixin, NameMixin, MyAbstractBaseModel):

                class Meta:
                    table = "role"

        Using the ``Meta`` class is not necessary. But it is a good habit, to
        give your table an explicit name. This way you can change the model name
        without breaking the schema. So the following definition is valid.

        .. code-block:: python3

            class RoleModel(TimestampMixin, NameMixin, MyAbstractBaseModel):
        pass

Meta类
-----

**The** ``Meta`` **class**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. autoclass:: tortoise.models.Model.Meta

            .. attribute:: abstract
                :annotation: = False

                设置为 ``True`` 表示这是一个抽象类。

            .. attribute:: schema
                :annotation: = ""

                设置此项以配置表所在的模式名称。

            .. attribute:: table
                :annotation: = ""

                设置此项以配置手动表名称，而不是生成的名称。

            .. attribute:: table_description
                :annotation: = ""

                设置此项以生成当前模型创建的表的注释信息。

            .. attribute:: unique_together
                :annotation: = None

                指定 ``unique_together`` 来为列集设置复合唯一索引。

                它应该是一个元组的元组（列表也可以），格式如下：

                .. code-block:: python3

                    unique_together=("field_a", "field_b")
                    unique_together=(("field_a", "field_b"), )
                    unique_together=(("field_a", "field_b"), ("field_c", "field_d", "field_e"))

            .. attribute:: indexes
                :annotation: = None

                指定 ``indexes`` 来为列集设置复合非唯一索引。

                它应该是一个元组的元组（列表也可以），格式如下：

                .. code-block:: python3

                    indexes=("field_a", "field_b")
                    indexes=(("field_a", "field_b"), )
                    indexes=(("field_a", "field_b"), ("field_c", "field_d", "field_e"))

            .. attribute:: ordering
                :annotation: = None

                指定 ``ordering`` 来为给定模型设置默认排序。
                它应该是可迭代的字符串，格式与 ``.order_by(...)`` 接收的格式相同。
                如果查询是使用 ``.annotate(...)`` 生成的 ``GROUP_BY`` 子句，则不应用默认排序。

                .. code-block:: python3

                    ordering = ["name", "-score"]

            .. attribute:: manager
                :annotation: = tortoise.manager.Manager

                指定 ``manager`` 以覆盖默认管理器。
                它应该是 ``tortoise.manager.Manager`` 或其子类的实例。

                .. code-block:: python3

                    manager = CustomManager()

    .. md-tab-item:: 英文

        .. autoclass:: tortoise.models.Model.Meta

            .. attribute:: abstract
                :annotation: = False

                Set to ``True`` to indicate this is an abstract class

            .. attribute:: schema
                :annotation: = ""

                Set this to configure a schema name, where table exists

            .. attribute:: table
                :annotation: = ""

                Set this to configure a manual table name, instead of a generated one

            .. attribute:: table_description
                :annotation: = ""

                Set this to generate a comment message for the table being created for the current model

            .. attribute:: unique_together
                :annotation: = None

                Specify ``unique_together`` to set up compound unique indexes for sets of columns.

                It should be a tuple of tuples (lists are fine) in the format of:

                .. code-block:: python3

                    unique_together=("field_a", "field_b")
                    unique_together=(("field_a", "field_b"), )
                    unique_together=(("field_a", "field_b"), ("field_c", "field_d", "field_e"))

            .. attribute:: indexes
                :annotation: = None

                Specify ``indexes`` to set up compound non-unique indexes for sets of columns.

                It should be a tuple of tuples (lists are fine) in the format of:

                .. code-block:: python3

                    indexes=("field_a", "field_b")
                    indexes=(("field_a", "field_b"), )
                    indexes=(("field_a", "field_b"), ("field_c", "field_d", "field_e"))

            .. attribute:: ordering
                :annotation: = None

                Specify ``ordering`` to set up default ordering for given model.
                It should be iterable of strings formatted in same way as ``.order_by(...)`` receives.
                If query is built with ``GROUP_BY`` clause using ``.annotate(...)`` default ordering is not applied.

                .. code-block:: python3

                    ordering = ["name", "-score"]

            .. attribute:: manager
                :annotation: = tortoise.manager.Manager

                Specify ``manager`` to override the default manager.
                It should be instance of ``tortoise.manager.Manager`` or subclass.

                .. code-block:: python3

                    manager = CustomManager()

外键
-------------------

``ForeignKeyField``

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: python3

            tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
            participants = fields.ManyToManyField('models.Team', related_name='events')
            modified = fields.DatetimeField(auto_now=True)
            prize = fields.DecimalField(max_digits=10, decimal_places=2, null=True)

        在事件模型中，我们添加了一些可能对我们有趣的字段。

        ``fields.ForeignKeyField('models.Tournament', related_name='events')``
            在这里，我们创建了一个指向锦标赛的外键引用。通过引用模型的字面量（由应用名称和模型名称组成）来创建。``models`` 是默认的应用名称，但可以在 ``class Meta`` 中使用 ``app = 'other'`` 来更改。

        ``related_name``
            是一个关键字参数，定义了对被引用模型的相关查询字段，因此你可以像这样获取所有锦标赛的事件：

        .. code-block:: python3

            await Tournament.first().prefetch_related("events")

    .. md-tab-item:: 英文

        .. code-block:: python3

            tournament = fields.ForeignKeyField('models.Tournament', related_name='events')
            participants = fields.ManyToManyField('models.Team', related_name='events')
            modified = fields.DatetimeField(auto_now=True)
            prize = fields.DecimalField(max_digits=10, decimal_places=2, null=True)

        In event model we got some more fields, that could be interesting for us.

        ``fields.ForeignKeyField('models.Tournament', related_name='events')``
            Here we create foreign key reference to tournament. We create it by referring to model by it's literal, consisting of app name and model name. ``models`` is default app name, but you can change it in ``class Meta`` with ``app = 'other'``.
        ``related_name``
            Is keyword argument, that defines field for related query on referenced models, so with that you could fetch all tournaments's events with like this:

        .. code-block:: python3

            await Tournament.first().prefetch_related("events")

数据库字段
^^^^^^^^^^^^^^^^^^^^

**The DB-backing field**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. note::

            ``ForeignKeyField`` 是一个虚拟字段，这意味着它没有直接的数据库支持。
            相反，它有一个字段（默认名为 :samp:`{FKNAME}_id`（即只附加 ``_id``）），
            这是实际的数据库支持字段。

            它只会包含相关表的键值。

            这是一个重要细节，因为这允许直接分配/读取实际值，
            如果不需要整个外部对象，这可以被视为一种优化。

        指定外键可以通过传递对象完成：

        .. code-block::  python3

            await SomeModel.create(tournament=the_tournament)
            # 或者
            somemodel.tournament=the_tournament

        或者通过直接访问数据库支持字段：

        .. code-block::  python3

            await SomeModel.create(tournament_id=the_tournament.pk)
            # 或者
            somemodel.tournament_id=the_tournament.pk

        查询关系通常通过追加双下划线，然后是外部对象的字段来完成。然后可以追加一个普通查询属性。
        如果下一个键也是一个外部对象，则可以链接：

            :samp:`{FKNAME}__{FOREIGNFIELD}__gt=3`

            或

            :samp:`{FKNAME}__{FOREIGNFK}__{VERYFOREIGNFIELD}__gt=3`

        然而，有一个主要的限制。我们不想限制外部列名称，或产生歧义（例如，一个外部对象可能有一个字段叫 ``isnull``）。

        那么这将是完全模糊的:

            :samp:`{FKNAME}__isnull`

        为了防止这种情况，我们要求直接过滤器应用于外键的数据库支持字段:

            :samp:`{FKNAME}_id__isnull`

    .. md-tab-item:: 英文

        .. note::

            A ``ForeignKeyField`` is a virtual field, meaning it has no direct DB backing.
            Instead it has a field (by default called :samp:`{FKNAME}_id` (that is, just an ``_id`` is appended)
            that is the actual DB-backing field.

            It will just contain the Key value of the related table.

            This is an important detail as it would allow one to assign/read the actual value directly,
            which could be considered an optimization if the entire foreign object isn't needed.


        Specifying an FK can be done via either passing the object:

        .. code-block::  python3

            await SomeModel.create(tournament=the_tournament)
            # or
            somemodel.tournament=the_tournament

        or by directly accessing the DB-backing field:

        .. code-block::  python3

            await SomeModel.create(tournament_id=the_tournament.pk)
            # or
            somemodel.tournament_id=the_tournament.pk


        Querying a relationship is typically done by appending a double underscore, and then the foreign object's field. Then a normal query attr can be appended.
        This can be chained if the next key is also a foreign object:

            :samp:`{FKNAME}__{FOREIGNFIELD}__gt=3`

            or

            :samp:`{FKNAME}__{FOREIGNFK}__{VERYFOREIGNFIELD}__gt=3`

        There is however one major limitation. We don't want to restrict foreign column names, or have ambiguity (e.g. a foreign object may have a field called ``isnull``)

        Then this would be entirely ambiguous:

            :samp:`{FKNAME}__isnull`

        To prevent that we require that direct filters be applied to the DB-backing field of the foreign key:

            :samp:`{FKNAME}_id__isnull`

获取外键对象
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Fetching the foreign object**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        获取外键可以通过异步和同步接口进行。

        异步获取：

        .. code-block:: python3

            events = await tournament.events.all()

        你可以像这样异步迭代它：

        .. code-block:: python3

            async for event in tournament.events:
                ...

        同步使用要求你在使用之前调用 `fetch_related`，然后你可以使用常见的函数，例如：

        .. code-block:: python3

            await tournament.fetch_related('events')
            events = list(tournament.events)
            eventlen = len(tournament.events)
            if SomeEvent in tournament.events:
                ...
            if tournament.events:
                ...
            firstevent = tournament.events[0]

        要获取反向外键，例如 `event.tournament`，我们目前只支持同步接口。

        .. code-block:: python3

            await event.fetch_related('tournament')
            tournament = event.tournament

    .. md-tab-item:: 英文

        Fetching foreign keys can be done with both async and sync interfaces.

        Async fetch:

        .. code-block:: python3

            events = await tournament.events.all()

        You can async iterate over it like this:

        .. code-block:: python3

            async for event in tournament.events:
                ...

        Sync usage requires that you call `fetch_related` before the time, and then you can use common functions such as:

        .. code-block:: python3

            await tournament.fetch_related('events')
            events = list(tournament.events)
            eventlen = len(tournament.events)
            if SomeEvent in tournament.events:
                ...
            if tournament.events:
                ...
            firstevent = tournament.events[0]


        To get the Reverse-FK, e.g. an `event.tournament` we currently only support the sync interface.

        .. code-block:: python3

            await event.fetch_related('tournament')
            tournament = event.tournament


多对多关系
-------------------

``ManyToManyField``

.. md-tab-set::
    
    .. md-tab-item:: 中文

        下一个字段是 ``fields.ManyToManyField('models.Team', related_name='events')``。它描述了与 Team 模型的多对多关系。

        要向 ``ManyToManyField`` 添加关系，两个模型都需要被保存，否则会引发 ``OperationalError``。

        解析多对多字段可以通过异步和同步接口进行。

        异步获取：

        .. code-block:: python3

            participants = await tournament.participants.all()

        你可以像这样异步迭代它：

        .. code-block:: python3

            async for participant in tournament.participants:
                ...

        同步使用要求你在使用之前调用 `fetch_related` ，然后你可以使用常见的函数，例如：

        .. code-block:: python3

            await tournament.fetch_related('participants')
            participants = list(tournament.participants)
            participantlen = len(tournament.participants)
            if SomeParticipant in tournament.participants:
                ...
            if tournament.participants:
                ...
            firstparticipant = tournament.participants[0]

        ``team.event_team`` 的反向查找工作方式完全相同。

    .. md-tab-item:: 英文

        Next field is ``fields.ManyToManyField('models.Team', related_name='events')``. It describes many to many relation to model Team.

        To add to a ``ManyToManyField`` both the models need to be saved, else you will get an ``OperationalError`` raised.

        Resolving many to many fields can be done with both async and sync interfaces.

        Async fetch:

        .. code-block:: python3

            participants = await tournament.participants.all()

        You can async iterate over it like this:

        .. code-block:: python3

            async for participant in tournament.participants:
                ...

        Sync usage requires that you call `fetch_related` before the time, and then you can use common functions such as:

        .. code-block:: python3

            await tournament.fetch_related('participants')
            participants = list(tournament.participants)
            participantlen = len(tournament.participants)
            if SomeParticipant in tournament.participants:
                ...
            if tournament.participants:
                ...
            firstparticipant = tournament.participants[0]

        The reverse lookup of ``team.event_team`` works exactly the same way.

改进关系类型提示
=================================

**Improving relational type hinting**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        由于 Tortoise ORM 仍然是一个年轻的项目，它在各种编辑器中的支持并不广泛，这些编辑器可以帮助你编写代码并提供良好的模型和不同关系的自动完成(自动提示)功能。然而，通过一些小的工作，你可以获得这样的自动补全。你需要做的就是为负责关系的字段向模型添加一些注解。

        下面是来自 :ref:`getting_started` 的更新示例，它将为所有模型添加自动完成(自动提示)功能，包括模型之间关系的字段。

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields


            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                events: fields.ReverseRelation["Event"]

                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)
                tournament: fields.ForeignKeyRelation[Tournament] = fields.ForeignKeyField(
                    "models.Tournament", related_name="events"
                )
                participants: fields.ManyToManyRelation["Team"] = fields.ManyToManyField(
                    "models.Team", related_name="events", through="event_team"
                )

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                events: fields.ManyToManyRelation[Event]

                def __str__(self):
                    return self.name

    .. md-tab-item:: 英文

        Since Tortoise ORM is still a young project, it does not have such widespread support by
        various editors who help you writing code using good autocomplete for models and
        different relations between them.
        However, you can get such autocomplete by doing a little work yourself.
        All you need to do is add a few annotations to your models for fields that are responsible
        for the relations.

        Here is an updated example from :ref:`getting_started`, that will add autocomplete for
        all models including fields for the relations between models.

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields


            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                events: fields.ReverseRelation["Event"]

                def __str__(self):
                    return self.name


            class Event(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)
                tournament: fields.ForeignKeyRelation[Tournament] = fields.ForeignKeyField(
                    "models.Tournament", related_name="events"
                )
                participants: fields.ManyToManyRelation["Team"] = fields.ManyToManyField(
                    "models.Team", related_name="events", through="event_team"
                )

                def __str__(self):
                    return self.name


            class Team(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

                events: fields.ManyToManyRelation[Event]

                def __str__(self):
                    return self.name


参考
=========

**Reference**

.. automodule:: tortoise.models
    :members: Model
    :undoc-members:
