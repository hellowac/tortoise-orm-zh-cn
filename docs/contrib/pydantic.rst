:tocdepth: 4

.. _contrib_pydantic:

======================
Pydantic 序列化
======================

**Pydantic serialisation**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 具有一个 Pydantic 插件，可以从 Tortoise 模型生成 Pydantic 模型，并提供辅助函数来序列化该模型及其相关对象。

        目前我们只支持为序列化生成 Pydantic 对象，尚不支持反序列化。

        请参阅 :ref:`examples_pydantic`。
    
    .. md-tab-item:: 英文

        Tortoise ORM has a Pydantic plugin that will generate Pydantic Models from Tortoise Models, and then provides helper functions to serialise that model and its related objects.

        We currently only support generating Pydantic objects for serialisation, and no deserialisation at this stage.

        See the :ref:`examples_pydantic`

.. rst-class:: emphasize-children

教程
========

**Tutorial**

.. rst-class:: html-toggle

1: 基本用法
--------------

**Basic usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在这里，我们介绍：

        * 从 Tortoise 模型创建 Pydantic 模型
        * 使用文档字符串和文档注释
        * 评估生成的架构
        * 使用 ``.model_dump()`` 和 ``.model_dump_json()`` 进行简单序列化

        示例源代码： :ref:`example_pydantic_tut1`

        让我们从一个基本的 Tortoise 模型开始：

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                这是一个比赛的引用
                """
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: 比赛记录创建的日期时间
                created_at = fields.DatetimeField(auto_now_add=True)

        | 要从中创建 Pydantic 模型，可以调用：
        | :meth:`tortoise.contrib.pydantic.creator.pydantic_model_creator`

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_model_creator

            Tournament_Pydantic = pydantic_model_creator(Tournament)

        现在我们有了一个 `Pydantic 模型 <https://pydantic-docs.helpmanual.io/usage/models/>`__，可以用于表示架构和序列化。

        ``Tournament_Pydantic`` 的 JSON 架构现在是：

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': '这是一个比赛的引用',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': '比赛记录创建的日期时间',
                        'type': 'string',
                        'format': 'date-time'
                    }
                }
            }

        注意类文档字符串和文档注释 ``#:`` 被包含为架构中的描述。

        要序列化一个对象，仅需 *(在异步上下文中)*：

        .. code-block:: py3

            tournament = await Tournament.create(name="新比赛")
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

        可以使用 `常规 Pydantic 对象方法 <https://pydantic-docs.helpmanual.io/usage/exporting_models/>`_ 获取内容，例如 ``.model_dump()`` 或 ``.model_dump_json()``。
    
    .. md-tab-item:: 英文

        Here we introduce:

        * Creating a Pydantic model from a Tortoise model
        * Docstrings & doc-comments are used
        * Evaluating the generated schema
        * Simple serialisation with both ``.model_dump()`` and ``.model_dump_json()``

        Source to example: :ref:`example_pydantic_tut1`

        Lets start with a basic Tortoise Model:

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                This references a Tournament
                """
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: The date-time the Tournament record was created at
                created_at = fields.DatetimeField(auto_now_add=True)

        | To create a Pydantic model from that one would call:
        | :meth:`tortoise.contrib.pydantic.creator.pydantic_model_creator`

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_model_creator

            Tournament_Pydantic = pydantic_model_creator(Tournament)

        And now have a `Pydantic Model <https://pydantic-docs.helpmanual.io/usage/models/>`__ that can be used for representing schema and serialisation.

        The JSON-Schema of ``Tournament_Pydantic`` is now:

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': 'This references a Tournament',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': 'The date-time the Tournament record was created at',
                        'type': 'string',
                        'format': 'date-time'
                    }
                }
            }

        Note how the class docstring and doc-comment ``#:`` is included as descriptions in the Schema.

        To serialise an object it is simply *(in an async context)*:

        .. code-block:: py3

            tournament = await Tournament.create(name="New Tournament")
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

        And one could get the contents by using `regular Pydantic-object methods <https://pydantic-docs.helpmanual.io/usage/exporting_models/>`_, such as ``.model_dump()`` or ``.model_dump_json()``

.. code-block:: py3

    >>> print(tourpy.model_dump())
    {
        'id': 1,
        'name': 'New Tournament',
        'created_at': datetime.datetime(2020, 3, 1, 20, 28, 9, 346808)
    }
    >>> print(tourpy.model_dump_json())
    {
        "id": 1,
        "name": "New Tournament",
        "created_at": "2020-03-01T20:28:09.346808"
    }


.. rst-class:: html-toggle

2: Queryset和List
--------------------

**Querysets & Lists**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在这里，我们介绍：

        * 创建一个列表模型以序列化查询集
        * 默认排序得以保留

        示例源代码： :ref:`example_pydantic_tut2`

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                这是一个比赛的引用
                """
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: 比赛记录创建的日期时间
                created_at = fields.DatetimeField(auto_now_add=True)

                class Meta:
                    # 定义默认排序
                    # Pydantic 序列化器将使用此排序结果
                    ordering = ["name"]

        | 要从中创建 Pydantic 列表模型，可以调用：
        | :meth:`tortoise.contrib.pydantic.creator.pydantic_queryset_creator`

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_queryset_creator

            Tournament_Pydantic_List = pydantic_queryset_creator(Tournament)

        现在我们有了一个 `Pydantic 模型 <https://pydantic-docs.helpmanual.io/usage/models/>`__，可以用于表示架构和序列化。

        ``Tournament_Pydantic_List`` 的 JSON 架构现在是：

        .. code-block:: py3

            >>> print(Tournament_Pydantic_List.schema())
            {
                'title': 'Tournaments',
                'description': '这是一个比赛的引用',
                'type': 'array',
                'items': {
                    '$ref': '#/definitions/Tournament'
                },
                'definitions': {
                    'Tournament': {
                        'title': 'Tournament',
                        'description': '这是一个比赛的引用',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'description': '比赛记录创建的日期时间',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        注意，``Tournament`` 现在不是根对象。一个简单的列表是根对象。

        要序列化一个对象，仅需 *(在异步上下文中)*：

        .. code-block:: py3

            # 创建对象
            await Tournament.create(name="新比赛")
            await Tournament.create(name="另一个")
            await Tournament.create(name="最后一个比赛")

            tourpy = await Tournament_Pydantic_List.from_queryset(Tournament.all())

        可以使用 `常规 Pydantic 对象方法 <https://pydantic-docs.helpmanual.io/usage/exporting_models/>`_ 获取内容，例如 ``.model_dump()`` 或 ``.model_dump_json()``。

        .. code-block:: py3

            >>> print(tourpy.model_dump())
            {
                'root': [
                    {
                        'id': 2,
                        'name': '另一个',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776504)
                    },
                    {
                        'id': 3,
                        'name': '最后一个比赛',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776848)
                    },
                    {
                        'id': 1,
                        'name': '新比赛',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776211)
                    }
                ]
            }
            >>> print(tourpy.model_dump_json())
            [
                {
                    "id": 2,
                    "name": "另一个",
                    "created_at": "2020-03-02T06:53:39.776504"
                },
                {
                    "id": 3,
                    "name": "最后一个比赛",
                    "created_at": "2020-03-02T06:53:39.776848"
                },
                {
                    "id": 1,
                    "name": "新比赛",
                    "created_at": "2020-03-02T06:53:39.776211"
                }
            ]

        注意，``.model_dump()`` 有一个 ``root`` 元素，包含列表，但 ``.model_dump_json()`` 将列表作为根元素。
        同时注意结果按 ``name`` 字母顺序排序。
    
    .. md-tab-item:: 英文

        Here we introduce:

        * Creating a list-model to serialise a queryset
        * Default sorting is honoured

        Source to example: :ref:`example_pydantic_tut2`

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                This references a Tournament
                """
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: The date-time the Tournament record was created at
                created_at = fields.DatetimeField(auto_now_add=True)

                class Meta:
                    # Define the default ordering
                    #  the pydantic serialiser will use this to order the results
                    ordering = ["name"]

        | To create a Pydantic list-model from that one would call:
        | :meth:`tortoise.contrib.pydantic.creator.pydantic_queryset_creator`

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_queryset_creator

            Tournament_Pydantic_List = pydantic_queryset_creator(Tournament)

        And now have a `Pydantic Model <https://pydantic-docs.helpmanual.io/usage/models/>`__ that can be used for representing schema and serialisation.

        The JSON-Schema of ``Tournament_Pydantic_List`` is now:

        .. code-block:: py3

            >>> print(Tournament_Pydantic_List.schema())
            {
                'title': 'Tournaments',
                'description': 'This references a Tournament',
                'type': 'array',
                'items': {
                    '$ref': '#/definitions/Tournament'
                },
                'definitions': {
                    'Tournament': {
                        'title': 'Tournament',
                        'description': 'This references a Tournament',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'description': 'The date-time the Tournament record was created at',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        Note that the ``Tournament`` is now not the root. A simple list is.

        To serialise an object it is simply *(in an async context)*:

        .. code-block:: py3

            # Create objects
            await Tournament.create(name="New Tournament")
            await Tournament.create(name="Another")
            await Tournament.create(name="Last Tournament")

            tourpy = await Tournament_Pydantic_List.from_queryset(Tournament.all())

        And one could get the contents by using `regular Pydantic-object methods <https://pydantic-docs.helpmanual.io/usage/exporting_models/>`_, such as ``.model_dump()`` or ``.model_dump_json()``

        .. code-block:: py3

            >>> print(tourpy.model_dump())
            {
                'root': [
                    {
                        'id': 2,
                        'name': 'Another',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776504)
                    },
                    {
                        'id': 3,
                        'name': 'Last Tournament',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776848)
                    },
                    {
                        'id': 1,
                        'name': 'New Tournament',
                        'created_at': datetime.datetime(2020, 3, 2, 6, 53, 39, 776211)
                    }
                ]
            }
            >>> print(tourpy.model_dump_json())
            [
                {
                    "id": 2,
                    "name": "Another",
                    "created_at": "2020-03-02T06:53:39.776504"
                },
                {
                    "id": 3,
                    "name": "Last Tournament",
                    "created_at": "2020-03-02T06:53:39.776848"
                },
                {
                    "id": 1,
                    "name": "New Tournament",
                    "created_at": "2020-03-02T06:53:39.776211"
                }
            ]

        Note how ``.model_dump()`` has a ``root`` element with the list, but the ``.model_dump_json()`` has the list as root.
        Also note how the results are sorted alphabetically by ``name``.


.. rst-class:: html-toggle

3: 关系和初始化
-------------------------

**Relations & Early-init**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在这里，我们介绍：

        * 关系
        * 早期模型初始化

        .. note::

            本教程关于早期初始化的部分仅在您需要在初始化 Tortoise ORM **之前** 生成 Pydantic 模型时才需要。

            请查看 :ref:`example_pydantic_basic` （在函数 ``run`` 中）以了解 ``*_creator`` 仅在我们正确初始化 Tortoise ORM 之后被调用，在这种情况下不需要早期初始化。

        示例源代码： :ref:`example_pydantic_tut3`

        我们定义带有关系的模型：

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                这是一个比赛的引用
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: 比赛记录创建的日期时间
                created_at = fields.DatetimeField(auto_now_add=True)

            class Event(Model):
                """
                这是比赛中的一个事件的引用
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                tournament = fields.ForeignKeyField(
                    "models.Tournament", related_name="events", description="该事件发生的比赛"
                )

        接下来，我们使用 ``pydantic_model_creator`` 创建我们的 `Pydantic 模型 <https://pydantic-docs.helpmanual.io/usage/models/>`__：

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_model_creator

            Tournament_Pydantic = pydantic_model_creator(Tournament)

        ``Tournament_Pydantic`` 的 JSON 架构现在是：

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': '这是一个比赛的引用',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': '比赛记录创建的日期时间',
                        'type': 'string',
                        'format': 'date-time'
                    }
                }
            }

        哦不！关系在哪里？

        因为模型尚未完全初始化，所以此时它不知道关系。

        我们需要使用 :meth:`tortoise.Tortoise.init_models` 早期初始化模型关系。

        .. code-block:: py3

            from tortoise import Tortoise

            Tortoise.init_models(["__main__"], "models")
            # 现在再试一次
            Tournament_Pydantic = pydantic_model_creator(Tournament)

        ``Tournament_Pydantic`` 的 JSON 架构现在是：

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': '这是一个比赛的引用',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': '比赛记录创建的日期时间',
                        'type': 'string',
                        'format': 'date-time'
                    },
                    'events': {
                        'title': 'Events',
                        'description': '该事件发生的比赛',
                        'type': 'array',
                        'items': {
                            '$ref': '#/definitions/Event'
                        }
                    }
                },
                'definitions': {
                    'Event': {
                        'title': 'Event',
                        'description': '这是比赛中的一个事件的引用',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        啊哈！这好得多。

        注意，我们也可以以相同的方式为 ``Event`` 创建一个模型，它应该可以正常工作：

        .. code-block:: py3

            Event_Pydantic = pydantic_model_creator(Event)

            >>> print(Event_Pydantic.schema())
            {
                'title': 'Event',
                'description': '这是比赛中的一个事件的引用',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'type': 'string',
                        'format': 'date-time'
                    },
                    'tournament': {
                        'title': 'Tournament',
                        'description': '该事件发生的比赛',
                        'allOf': [
                            {
                                '$ref': '#/definitions/Tournament'
                            }
                        ]
                    }
                },
                'definitions': {
                    'Tournament': {
                        'title': 'Tournament',
                        'description': '这是一个比赛的引用',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'description': '比赛记录创建的日期时间',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        并且也定义了关系！

        注意，这两个架构都没有反向跟随关系。这是默认设置，稍后的教程中我们将展示相关选项。

        让我们创建并序列化对象，看看它们的样子 *(在异步上下文中)*：

        .. code-block:: py3

            # 创建对象
            tournament = await Tournament.create(name="新比赛")
            event = await Event.create(name="事件", tournament=tournament)

            # 序列化比赛
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

            >>> print(tourpy.model_dump_json())
            {
                "id": 1,
                "name": "新比赛",
                "created_at": "2020-03-02T07:23:27.731656",
                "events": [
                    {
                        "id": 1,
                        "name": "事件",
                        "created_at": "2020-03-02T07:23:27.732492"
                    }
                ]
            }

        并序列化事件 *(在异步上下文中)*：

        .. code-block:: py3

            eventpy = await Event_Pydantic.from_tortoise_orm(event)

            >>> print(eventpy.model_dump_json())
            {
                "id": 1,
                "name": "事件",
                "created_at": "2020-03-02T07:23:27.732492",
                "tournament": {
                    "id": 1,
                    "name": "新比赛",
                    "created_at": "2020-03-02T07:23:27.731656"
                }
            }

    .. md-tab-item:: 英文

        Here we introduce:

        * Relationships
        * Early model init

        .. note::

            The part of this tutorial about early-init is only required if you need to generate the pydantic models **before** you have initialised Tortoise ORM.

            Look at :ref:`example_pydantic_basic` (in function ``run``) to see where the ``*_creator is only`` called **after** we initialised Tortoise ORM properly, in that case an early init is not needed.

        Source to example: :ref:`example_pydantic_tut3`

        We define our models with a relationship:

        .. code-block:: py3

            from tortoise import fields
            from tortoise.models import Model

            class Tournament(Model):
                """
                This references a Tournament
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                #: The date-time the Tournament record was created at
                created_at = fields.DatetimeField(auto_now_add=True)

            class Event(Model):
                """
                This references an Event in a Tournament
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                tournament = fields.ForeignKeyField(
                    "models.Tournament", related_name="events", description="The Tournament this happens in"
                )

        Next we create our `Pydantic Model <https://pydantic-docs.helpmanual.io/usage/models/>`__ using ``pydantic_model_creator``:

        .. code-block:: py3

            from tortoise.contrib.pydantic import pydantic_model_creator

            Tournament_Pydantic = pydantic_model_creator(Tournament)

        The JSON-Schema of ``Tournament_Pydantic`` is now:

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': 'This references a Tournament',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': 'The date-time the Tournament record was created at',
                        'type': 'string',
                        'format': 'date-time'
                    }
                }
            }

        Oh no! Where is the relation?

        Because the models have not fully initialised, it doesn't know about the relations at this stage.

        We need to initialise our model relationships early using :meth:`tortoise.Tortoise.init_models`

        .. code-block:: py3

            from tortoise import Tortoise

            Tortoise.init_models(["__main__"], "models")
            # Now lets try again
            Tournament_Pydantic = pydantic_model_creator(Tournament)

        The JSON-Schema of ``Tournament_Pydantic`` is now:

        .. code-block:: py3

            >>> print(Tournament_Pydantic.schema())
            {
                'title': 'Tournament',
                'description': 'This references a Tournament',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'description': 'The date-time the Tournament record was created at',
                        'type': 'string',
                        'format': 'date-time'
                    },
                    'events': {
                        'title': 'Events',
                        'description': 'The Tournament this happens in',
                        'type': 'array',
                        'items': {
                            '$ref': '#/definitions/Event'
                        }
                    }
                },
                'definitions': {
                    'Event': {
                        'title': 'Event',
                        'description': 'This references an Event in a Tournament',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        Aha! that's much better.

        Note we can also create a model for ``Event`` the same way, and it should just work:

        .. code-block:: py3

            Event_Pydantic = pydantic_model_creator(Event)

            >>> print(Event_Pydantic.schema())
            {
                'title': 'Event',
                'description': 'This references an Event in a Tournament',
                'type': 'object',
                'properties': {
                    'id': {
                        'title': 'Id',
                        'type': 'integer'
                    },
                    'name': {
                        'title': 'Name',
                        'type': 'string'
                    },
                    'created_at': {
                        'title': 'Created At',
                        'type': 'string',
                        'format': 'date-time'
                    },
                    'tournament': {
                        'title': 'Tournament',
                        'description': 'The Tournament this happens in',
                        'allOf': [
                            {
                                '$ref': '#/definitions/Tournament'
                            }
                        ]
                    }
                },
                'definitions': {
                    'Tournament': {
                        'title': 'Tournament',
                        'description': 'This references a Tournament',
                        'type': 'object',
                        'properties': {
                            'id': {
                                'title': 'Id',
                                'type': 'integer'
                            },
                            'name': {
                                'title': 'Name',
                                'type': 'string'
                            },
                            'created_at': {
                                'title': 'Created At',
                                'description': 'The date-time the Tournament record was created at',
                                'type': 'string',
                                'format': 'date-time'
                            }
                        }
                    }
                }
            }

        And that also has the relation defined!

        Note how both schema's don't follow relations back. This is on by default, and in a later tutorial we will show the options.

        Lets create and serialise the objects and see what they look like *(in an async context)*:

        .. code-block:: py3

            # Create objects
            tournament = await Tournament.create(name="New Tournament")
            event = await Event.create(name="The Event", tournament=tournament)

            # Serialise Tournament
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

            >>> print(tourpy.model_dump_json())
            {
                "id": 1,
                "name": "New Tournament",
                "created_at": "2020-03-02T07:23:27.731656",
                "events": [
                    {
                        "id": 1,
                        "name": "The Event",
                        "created_at": "2020-03-02T07:23:27.732492"
                    }
                ]
            }

        And serialising the event *(in an async context)*:

        .. code-block:: py3

            eventpy = await Event_Pydantic.from_tortoise_orm(event)

            >>> print(eventpy.model_dump_json())
            {
                "id": 1,
                "name": "The Event",
                "created_at": "2020-03-02T07:23:27.732492",
                "tournament": {
                    "id": 1,
                    "name": "New Tournament",
                    "created_at": "2020-03-02T07:23:27.731656"
                }
            }


.. rst-class:: html-toggle

4: PydanticMeta 和 Callables
---------------------------

**PydanticMeta & Callables**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在这里，我们介绍：

        * 通过 ``PydanticMeta`` 类配置模型创建器。
        * 使用可调用函数注释额外数据。

        示例源代码： :ref:`example_pydantic_tut4`

        让我们添加一些计算数据的方法，并告诉创建器使用它们：

        .. code-block:: py3

            class Tournament(Model):
                """
                这是一个比赛的引用
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                # 手动定义反向关系是有用的，这样类型检查
                #  和自动完成才能正常工作
                events: fields.ReverseRelation["Event"]

                def name_length(self) -> int:
                    """
                    计算名称的长度
                    """
                    return len(self.name)

                def events_num(self) -> int:
                    """
                    计算事件数量
                    """
                    try:
                        return len(self.events)
                    except NoValuesFetched:
                        return -1

                class PydanticMeta:
                    # 排除创建时间戳
                    exclude = ("created_at",)
                    # 包含两个可调用函数作为计算列
                    computed = ("name_length", "events_num")


            class Event(Model):
                """
                这是比赛中的一个事件的引用
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                tournament = fields.ForeignKeyField(
                    "models.Tournament", related_name="events", description="该事件发生的比赛"
                )

                class Meta:
                    ordering = ["name"]

                class PydanticMeta:
                    exclude = ("created_at",)

        这里有很多内容需要解释。

        首先，我们定义了一个 ``PydanticMeta`` 块，里面是 Pydantic 模型创建器的配置选项。
        请参见 :class:`tortoise.contrib.pydantic.creator.PydanticMeta` 以了解可用选项。

        其次，我们在两个模型中都排除了 ``created_at``，因为我们认为它没有提供任何好处。

        第三，我们添加了两个可调用函数： ``name_length`` 和 ``events_num``。我们希望这些成为结果集的一部分。
        请注意，可调用函数/计算字段需要手动指定返回类型，因为如果没有这个，我们无法确定创建有效 Pydantic 架构所需的记录类型。
        这对于标准 Tortoise ORM 字段来说是不需要的，因为字段已经定义了有效的类型。

        请注意，Pydantic 序列化器不能调用异步方法，但由于 Tortoise 辅助函数预先获取关系数据，因此在序列化之前可以使用它。
        所以我们不需要等待关系。
        然而，我们应该防止没有预获取的情况，因此需要捕获和处理 ``tortoise.exceptions.NoValuesFetched`` 异常。

        接下来，我们使用 ``pydantic_model_creator`` 创建我们的 `Pydantic 模型 <https://pydantic-docs.helpmanual.io/usage/models/>`__：

        .. code-block:: py3

            from tortoise import Tortoise

            Tortoise.init_models(["__main__"], "models")
            Tournament_Pydantic = pydantic_model_creator(Tournament)

        ``Tournament_Pydantic`` 的 JSON 架构现在是：

        .. code-block:: json

            {
                "title": "Tournament",
                "description": "这是一个比赛的引用",
                "type": "object",
                "properties": {
                    "id": {
                        "title": "Id",
                        "type": "integer"
                    },
                    "name": {
                        "title": "Name",
                        "type": "string"
                    },
                    "events": {
                        "title": "Events",
                        "description": "该事件发生的比赛",
                        "type": "array",
                        "items": {
                            "$ref": "#/definitions/Event"
                        }
                    },
                    "name_length": {
                        "title": "Name Length",
                        "description": "计算名称的长度",
                        "type": "integer"
                    },
                    "events_num": {
                        "title": "Events Num",
                        "description": "计算事件数量。",
                        "type": "integer"
                    }
                },
                "definitions": {
                    "Event": {
                        "title": "Event",
                        "description": "这是比赛中的一个事件的引用",
                        "type": "object",
                        "properties": {
                            "id": {
                                "title": "Id",
                                "type": "integer"
                            },
                            "name": {
                                "title": "Name",
                                "type": "string"
                            }
                        }
                    }
                }
            }

        注意 ``created_at`` 被移除，``name_length`` 和 ``events_num`` 被添加。

        让我们创建并序列化对象，看看它们的样子 *(在异步上下文中)*：

        .. code-block:: py3

            # 创建对象
            tournament = await Tournament.create(name="新比赛")
            await Event.create(name="事件 1", tournament=tournament)
            await Event.create(name="事件 2", tournament=tournament)

            # 序列化比赛
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

            >>> print(tourpy.model_dump_json())
            {
                "id": 1,
                "name": "新比赛",
                "events": [
                    {
                        "id": 1,
                        "name": "事件 1"
                    },
                    {
                        "id": 2,
                        "name": "事件 2"
                    }
                ],
                "name_length": 14,
                "events_num": 2
            }

    .. md-tab-item:: 英文

        Here we introduce:

        * Configuring model creator via ``PydanticMeta`` class.
        * Using callable functions to annotate extra data.

        Source to example: :ref:`example_pydantic_tut4`

        Let's add some methods that calculate data, and tell the creators to use them:

        .. code-block:: py3

            class Tournament(Model):
                """
                This references a Tournament
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                # It is useful to define the reverse relations manually so that type checking
                #  and auto completion work
                events: fields.ReverseRelation["Event"]

                def name_length(self) -> int:
                    """
                    Computed length of name
                    """
                    return len(self.name)

                def events_num(self) -> int:
                    """
                    Computed team size
                    """
                    try:
                        return len(self.events)
                    except NoValuesFetched:
                        return -1

                class PydanticMeta:
                    # Let's exclude the created timestamp
                    exclude = ("created_at",)
                    # Let's include two callables as computed columns
                    computed = ("name_length", "events_num")


            class Event(Model):
                """
                This references an Event in a Tournament
                """

                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=100)
                created_at = fields.DatetimeField(auto_now_add=True)

                tournament = fields.ForeignKeyField(
                    "models.Tournament", related_name="events", description="The Tournament this happens in"
                )

                class Meta:
                    ordering = ["name"]

                class PydanticMeta:
                    exclude = ("created_at",)

        There is much to unpack here.

        Firstly, we defined a ``PydanticMeta`` block, and in there is configuration options for the pydantic model creator.
        See :class:`tortoise.contrib.pydantic.creator.PydanticMeta` for the available options.

        Secondly, we excluded ``created_at`` in both models, as we decided it provided no benefit.

        Thirly, we added two callables: ``name_length`` and ``events_num``. We want these as part of the result set.
        Note that callables/computed fields require manual specification of return type, as without this we can't determine the record type which is needed to create a valid Pydantic schema.
        This is not needed for standard Tortoise ORM fields, as the fields already define a valid type.

        Note that the Pydantic serializer can't call async methods, but since the tortoise helpers pre-fetch relational data, it is available before serialization.
        So we don't need to await the relation.
        We should however protect against the case where no prefetching was done, hence catching and handling the ``tortoise.exceptions.NoValuesFetched`` exception.

        Next we create our `Pydantic Model <https://pydantic-docs.helpmanual.io/usage/models/>`__ using ``pydantic_model_creator``:

        .. code-block:: py3

            from tortoise import Tortoise

            Tortoise.init_models(["__main__"], "models")
            Tournament_Pydantic = pydantic_model_creator(Tournament)

        The JSON-Schema of ``Tournament_Pydantic`` is now:

        .. code-block:: json

            {
                "title": "Tournament",
                "description": "This references a Tournament",
                "type": "object",
                "properties": {
                    "id": {
                        "title": "Id",
                        "type": "integer"
                    },
                    "name": {
                        "title": "Name",
                        "type": "string"
                    },
                    "events": {
                        "title": "Events",
                        "description": "The Tournament this happens in",
                        "type": "array",
                        "items": {
                            "$ref": "#/definitions/Event"
                        }
                    },
                    "name_length": {
                        "title": "Name Length",
                        "description": "Computes length of name",
                        "type": "integer"
                    },
                    "events_num": {
                        "title": "Events Num",
                        "description": "Computes team size.",
                        "type": "integer"
                    }
                },
                "definitions": {
                    "Event": {
                        "title": "Event",
                        "description": "This references an Event in a Tournament",
                        "type": "object",
                        "properties": {
                            "id": {
                                "title": "Id",
                                "type": "integer"
                            },
                            "name": {
                                "title": "Name",
                                "type": "string"
                            }
                        }
                    }
                }
            }

        Note that ``created_at`` is removed, and ``name_length`` & ``events_num`` is added.

        Lets create and serialise the objects and see what they look like *(in an async context)*:

        .. code-block:: py3

            # Create objects
            tournament = await Tournament.create(name="New Tournament")
            await Event.create(name="Event 1", tournament=tournament)
            await Event.create(name="Event 2", tournament=tournament)

            # Serialise Tournament
            tourpy = await Tournament_Pydantic.from_tortoise_orm(tournament)

            >>> print(tourpy.model_dump_json())
            {
                "id": 1,
                "name": "New Tournament",
                "events": [
                    {
                        "id": 1,
                        "name": "Event 1"
                    },
                    {
                        "id": 2,
                        "name": "Event 2"
                    }
                ],
                "name_length": 14,
                "events_num": 2
            }



创建器
========

**Creators**

.. automodule:: tortoise.contrib.pydantic.creator
    :members:
    :exclude-members: PydanticMeta

PydanticMeta
============

.. autoclass:: tortoise.contrib.pydantic.creator.PydanticMeta
    :members:

模型类
=============

**Model classes**

.. automodule:: tortoise.contrib.pydantic.base
    :members:
