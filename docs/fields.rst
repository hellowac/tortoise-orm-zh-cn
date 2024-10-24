:tocdepth: 4

.. _fields:

======
字段
======


**Fields**

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        字段被定义为 ``Model`` 类对象的属性：

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)

    .. md-tab-item:: 英文

        Fields are defined as properties of a ``Model`` class object:

        .. code-block:: python3

            from tortoise.models import Model
            from tortoise import fields

            class Tournament(Model):
                id = fields.IntField(primary_key=True)
                name = fields.CharField(max_length=255)


.. rst-class:: emphasize-children

参考
=========

**Reference**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        以下是可用字段的列表以及这些字段的自定义选项：
    
    .. md-tab-item:: 英文

        Here is the list of fields available with custom options of these fields:

基本字段
----------

**Base Field**

.. automodule:: tortoise.fields.base
    :members:
    :undoc-members:
    :exclude-members: field_type, indexable, has_db_field, skip_to_python_if_native, allows_generated, function_cast, SQL_TYPE, GENERATED_SQL

数据字段
-----------

**Data Fields**

.. automodule:: tortoise.fields.data
    :members:
    :exclude-members: to_db_value, to_python_value

关系字段
-----------------

**Relational Fields**

.. automodule:: tortoise.fields.relational
    :members: ForeignKeyField, OneToOneField, ManyToManyField
    :exclude-members: to_db_value, to_python_value

DB特定字段
------------------

**DB Specific Fields**

MySQL
^^^^^

.. automodule:: tortoise.contrib.mysql.fields
    :members: GeometryField, UUIDField

Postgres
^^^^^^^^

.. automodule:: tortoise.contrib.postgres.fields
    :members: TSVectorField

扩展一个字段
=================

**Extending A Field**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        可以通过子类化字段来使用任意类型，只要它们可以以与数据库兼容的格式表示。一个示例是简单地封装 :class:`~tortoise.fields.CharField` 以存储和查询枚举类型。

        .. code-block:: python3

            from enum import Enum
            from typing import Type

            from tortoise import ConfigurationError
            from tortoise.fields import CharField


            class EnumField(CharField):
                """
                一个扩展 CharField 的示例，它将枚举序列化为
                数据库中的字符串表示形式。
                """

                def __init__(self, enum_type: Type[Enum], **kwargs):
                    super().__init__(128, **kwargs)
                    if not issubclass(enum_type, Enum):
                        raise ConfigurationError("{} 不是 Enum 的子类！".format(enum_type))
                    self._enum_type = enum_type

                def to_db_value(self, value: Enum, instance) -> str:
                    return value.value

                def to_python_value(self, value: str) -> Enum:
                    try:
                        return self._enum_type(value)
                    except Exception:
                        raise ValueError(
                            "数据库值 {} 在枚举 {} 中不存在。".format(value, self._enum_type)
                        )

        子类化时，请确保 ``to_db_value`` 返回与父类相同的类型（在 CharField 的情况下，即 ``str``），并且自然地，``to_python_value`` 接受值参数中相同类型（也是 ``str``）。

    .. md-tab-item:: 英文

        It is possible to subclass fields allowing use of arbitrary types as long as they can be represented in a
        database compatible format. An example of this would be a simple wrapper around the :class:`~tortoise.fields.CharField`
        to store and query Enum types.

        .. code-block:: python3

            from enum import Enum
            from typing import Type

            from tortoise import ConfigurationError
            from tortoise.fields import CharField


            class EnumField(CharField):
                """
                An example extension to CharField that serializes Enums
                to and from a str representation in the DB.
                """

                def __init__(self, enum_type: Type[Enum], **kwargs):
                    super().__init__(128, **kwargs)
                    if not issubclass(enum_type, Enum):
                        raise ConfigurationError("{} is not a subclass of Enum!".format(enum_type))
                    self._enum_type = enum_type

                def to_db_value(self, value: Enum, instance) -> str:
                    return value.value

                def to_python_value(self, value: str) -> Enum:
                    try:
                        return self._enum_type(value)
                    except Exception:
                        raise ValueError(
                            "Database value {} does not exist on Enum {}.".format(value, self._enum_type)
                        )

        When subclassing, make sure that the ``to_db_value`` returns the same type as the superclass (in the case of CharField,
        that is a ``str``) and that, naturally, ``to_python_value`` accepts the same type in the value parameter (also ``str``).
