:tocdepth: 3

.. _expressions:

===========
表达式
===========

**Expressions**

Q表达式
============

**Q Expression**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        有时，您需要进行比简单的 AND ``<model>.filter()`` 提供的更复杂的查询。幸运的是，我们有 Q 对象来增强查询功能，并帮助您找到所需内容。这些 Q 对象可以作为参数传递给 ``<model>.filter()``。

        Q 对象非常灵活，一些使用案例包括：
        - 创建 OR 过滤器
        - 嵌套过滤器
        - 反向过滤器
        - 结合以上任意组合以简单地编写复杂的多层过滤器

        Q 对象可以接受任何（特殊）关键字参数，用于过滤 ``<model>.filter()`` 接受的选项，具体的过滤选项请参见相关文档。

        它们也可以通过位运算符进行组合（ ``|`` 表示 OR， ``&`` 表示 AND，适合不熟悉位运算符的用户）。

        例如，要查找名称为 ``Event 1`` 或 ``Event 2`` 的事件：

        .. code-block:: python3

            found_events = await Event.filter(
                Q(name='Event 1') | Q(name='Event 2')
            )

        Q 对象也可以嵌套，上述示例等效于：

        .. code-block:: python3

            found_events = await Event.filter(
                Q(Q(name='Event 1'), Q(name='Event 2'), join_type="OR")
            )

        如果省略连接类型，则默认为 ``AND``。

        .. note::
            没有过滤参数的 Q 对象被视为无操作（NOP），并将在最终查询中被忽略（无论它们是用作 ``AND`` 还是 ``OR`` 参数）。

        此外，Q 对象支持取反，以生成查询中的 ``NOT`` （ ``~`` 操作符）子句。

        .. code-block:: python3

            not_third_events = await Event.filter(~Q(name='3'))
    
    .. md-tab-item:: 英文

        Sometimes you need to do more complicated queries than the simple AND ``<model>.filter()`` provides. Luckily we have Q objects to spice things up and help you find what you need. These Q-objects can then be used as argument to ``<model>.filter()`` instead.

        Q objects are extremely versatile, some example use cases:
        - creating an OR filter
        - nested filters
        - inverted filters
        - combining any of the above to simply write complicated multilayer filters

        Q objects can take any (special) kwargs for filtering that ``<model>.filter()`` accepts, see those docs for a full list of filter options in that regard.

        They can also be combined by using bitwise operators (``|`` is OR and ``&`` is AND for those unfamiliar with bitwise operators)

        For example to find the events with as name ``Event 1`` or ``Event 2``:

        .. code-block:: python3

            found_events = await Event.filter(
                Q(name='Event 1') | Q(name='Event 2')
            )

        Q objects can be nested as well, the above for example is equivalent to:

        .. code-block:: python3

            found_events = await Event.filter(
                Q(Q(name='Event 1'), Q(name='Event 2'), join_type="OR")
            )

        If join type is omitted it defaults to ``AND``.

        .. note::
            Q objects without filter arguments are considered NOP and will be ignored for the final query (regardless on if they are used as ``AND`` or ``OR`` param)


        Also, Q objects support negated to generate ``NOT`` (``~`` operator) clause in your query

        .. code-block:: python3

            not_third_events = await Event.filter(~Q(name='3'))

.. automodule:: tortoise.expressions
    :members: Q
    :undoc-members:

F 表达式
============

**F Expression**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        `F` 对象表示模型字段的值。它使得可以引用模型字段的值并使用它们执行数据库操作，而无需将它们从数据库提取到 Python 内存中。

        例如，使用 ``F`` 来原子性地更新用户余额：

        .. code-block:: python3

            from tortoise.expressions import F

            await User.filter(id=1).update(balance=F('balance') - 10)
            await User.filter(id=1).update(balance=F('balance') + F('award'), award=0)

            # 或使用 .save()
            user = await User.get(id=1)
            user.balance = F('balance') - 10
            await user.save(update_fields=['balance'])

        在此，如果您想再次访问更新后的 `F` 字段，应该首先调用 `refresh_from_db` 来刷新特定字段。

        .. code-block:: python3

            # 不能这样做！
            balance = user.balance
            await user.refresh_from_db(fields=['balance'])
            # 太好了！
            balance = user.balance

        您还可以在 `annotate` 中使用 `F`。

        .. code-block:: python3

            data = await User.annotate(idp=F("id") + 1).values_list("id", "idp")

    .. md-tab-item:: 英文

        An `F` object represents the value of a model field. It makes it possible to refer to model field values and perform database operations using them without actually having to pull them out of the database into Python memory.

        For example to use ``F`` to update user balance atomic:

        .. code-block:: python3

            from tortoise.expressions import F

            await User.filter(id=1).update(balance = F('balance') - 10)
            await User.filter(id=1).update(balance = F('balance') + F('award'), award = 0)

            # or use .save()
            user = await User.get(id=1)
            user.balance = F('balance') - 10
            await user.save(update_fields=['balance'])

        For this if you want access updated `F` field again, you should call `refresh_from_db` to refresh special fields first.

        .. code-block:: python3

            # Can't do this!
            balance = user.balance
            await user.refresh_from_db(fields=['balance'])
            # Great!
            balance = user.balance

        And you can also use `F` in `annotate`.

        .. code-block:: python3

            data = await User.annotate(idp=F("id") + 1).values_list("id", "idp")

子查询
========

**Subquery**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        你可以在 `filter()` 和 `annotate()` 中使用子查询( `Subquery` ).

        .. code-block:: python3

            from tortoise.expressions import Subquery

            await Tournament.annotate(ids=Subquery(Tournament.all().limit(1).values("id"))).values("ids", "id")
            await Tournament.filter(pk=Subquery(Tournament.filter(pk=t1.pk).values("id"))).first()

    .. md-tab-item:: 英文

        You can use `Subquery` in `filter()` and `annotate()`.

        .. code-block:: python3

            from tortoise.expressions import Subquery

            await Tournament.annotate(ids=Subquery(Tournament.all().limit(1).values("id"))).values("ids", "id")
            await Tournament.filter(pk=Subquery(Tournament.filter(pk=t1.pk).values("id"))).first()

原生SQL
======

**RawSQL**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        `RawSQL` 类似于 `Subquery`，但提供了编写原始 SQL 的能力。

        您可以在 `filter()` 和 `annotate()` 中使用 `RawSQL`。
    
    .. md-tab-item:: 英文

        `RawSQL` just like `Subquery` but provides the ability to write raw sql.

        You can use `RawSQL` in `filter()` and `annotate()`.

.. code-block:: python3

    await Tournament.filter(pk=1).annotate(count=RawSQL('count(*)')).values("count")
    await Tournament.filter(pk=1).annotate(idp=RawSQL('id + 1')).filter(idp=2).values("idp")
    await Tournament.filter(pk=RawSQL("id + 1"))


Case-When 表达式
====================

**Case-When Expression**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        构建经典的 `CASE WHEN ... THEN ... ELSE ... END` sql 代码片段。
    
    .. md-tab-item:: 英文

        Build classic `CASE WHEN ... THEN ... ELSE ... END` sql snippet.

.. autoclass:: tortoise.expressions.When

.. autoclass:: tortoise.expressions.Case

.. code-block:: py3

    results = await IntModel.all().annotate(category=Case(When(intnum__gte=8, then='big'), When(intnum__lte=2, then='small'), default='middle'))
