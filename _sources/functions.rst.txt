:tocdepth: 3

.. _functions:

======================
函数和聚合
======================

**Functions & Aggregates**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        要对值应用函数并获取在数据库端计算的聚合，需要对 QuerySet 进行注释。

        .. code-block:: python3

            results = await SomeModel.filter(...).annotate(clean_desc=Coalesce("desc", "N/A"))

        这将为每个 ``SomeModel`` 实例添加一个新属性 ``clean_desc``，该属性现在将包含注释的数据。

        还可以在其上调用 ``.values()`` 或 ``.values_list()`` 来按照常规方式获取数据。

    .. md-tab-item:: 英文

        To apply functions to values and get aggregates computed on the DB side, one needs to annotate the QuerySet.

        .. code-block:: py3

            results = await SomeModel.filter(...).annotate(clean_desc=Coalesce("desc", "N/A"))

        This will add a new attribute on each ``SomeModel`` instance called ``clean_desc`` that will now contain the annotated data.

        One can also call ``.values()`` or ``.values_list()`` on it to get the data as per regular.

函数
=========

**Functions**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        函数对字段的每个实例应用转换。
    
    .. md-tab-item:: 英文

        Functions apply a transform on each instance of a Field.

.. autoclass:: tortoise.functions.Trim

.. autoclass:: tortoise.functions.Length

.. autoclass:: tortoise.functions.Coalesce

.. autoclass:: tortoise.functions.Lower

.. autoclass:: tortoise.functions.Upper

.. autoclass:: tortoise.functions.Concat

.. autoclass:: tortoise.contrib.mysql.functions.Rand

.. autoclass:: tortoise.contrib.postgres.functions.Random

.. autoclass:: tortoise.contrib.sqlite.functions.Random

聚合
==========

**Aggregates**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        聚合应用于整个列，并且经常与分组一起使用。因此通常使用 ``.first()`` QuerySet 才有意义。
    
    .. md-tab-item:: 英文

        Aggregated apply on the entire column, and will often be used with grouping.
        So often makes sense with a ``.first()`` QuerySet.

.. autoclass:: tortoise.functions.Count

.. autoclass:: tortoise.functions.Sum

.. autoclass:: tortoise.functions.Max

.. autoclass:: tortoise.functions.Min

.. autoclass:: tortoise.functions.Avg


基础函数类
===================

**Base function class**

.. automodule:: tortoise.functions
    :members: Aggregate

    .. autoclass:: Function
        :members:

自定义函数
================

**Custom functions**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        您可以定义自定义函数，这些函数不是内置的，例如 ``TruncMonth`` 和 ``JsonExtract`` 等。

        .. code-block:: python3

            from pypika import CustomFunction
            from tortoise.expressions import F, Function

            class TruncMonth(Function):
                database_func = CustomFunction("DATE_FORMAT", ["name", "dt_format"])

            sql = Task.all().annotate(date=TruncMonth('created_at', '%Y-%m-%d')).values('date').sql()
            print(sql)
            # SELECT DATE_FORMAT(`created_at`,'%Y-%m-%d') `date` FROM `task`

        您还可以在更新中使用函数，该示例仅适用于 MySQL 和 SQLite，但 PostgreSQL 也是一样的。
    
    .. md-tab-item:: 英文

        You can define custom functions which are not builtin, such as ``TruncMonth`` and ``JsonExtract`` etc.

        .. code-block:: python3

            from pypika import CustomFunction
            from tortoise.expressions import F, Function

            class TruncMonth(Function):
                database_func = CustomFunction("DATE_FORMAT", ["name", "dt_format"])

            sql = Task.all().annotate(date=TruncMonth('created_at', '%Y-%m-%d')).values('date').sql()
            print(sql)
            # SELECT DATE_FORMAT(`created_at`,'%Y-%m-%d') `date` FROM `task`

        And you can also use functions in update, the example is only suitable for MySQL and SQLite, but PostgreSQL is the same.

.. code-block:: python3

    from tortoise.expressions import F
    from pypika.terms import Function

    class JsonSet(Function):
        def __init__(self, field: F, expression: str, value: Any):
            super().__init__("JSON_SET", field, expression, value)

    json = await JSONFields.create(data_default={"a": 1})
    json.data_default = JsonSet(F("data_default"), "$.a", 2)
    await json.save()

    # or use queryset.update()
    sql = JSONFields.filter(pk=json.pk).update(data_default=JsonSet(F("data_default"), "$.a", 3)).sql()
    print(sql)
    # UPDATE jsonfields SET data_default=JSON_SET(`data_default`,'$.a',3) where id=1
