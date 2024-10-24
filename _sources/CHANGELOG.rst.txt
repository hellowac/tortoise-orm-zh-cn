.. _changelog:

=========
变更日志
=========

**Changelog**

.. rst-class:: emphasize-children

0.21
====

0.21.7
------

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复与 pydantic2.9 相关的单元测试错误 (#1734)
        - 修复在同时使用 annotate 和 count 时，如果注解不匹配任何内容而导致的 IndexError (#1707)
        - 添加缺失的 TimeDeltaField 的 field_type (#1462) (#1699)
        - 改进 jsonfield 的类型提示 (#1700)
        - 修复 tortoise.models.Model 中的错误：当 QuerySet 使用 only 函数后再使用 print 函数打印返回结果时，会生成 AttributeError (#1724)
        - 更新 pylint 插件至最新的 astroid 版本 (#1708)
    
    .. md-tab-item:: 英文

        - Fix unittest error with pydantic2.9 (#1734)
        - Fix bug when using annotate and count at the same time but the annotation does not match anything, leading to an IndexError (#1707)
        - Added missing field_type for TimeDeltaField (#1462) (#1699)
        - improve jsonfield type hint (#1700)
        - Fix bug in tortoise.models.Model When a QuerySet uses the only function and then uses the print function to print the returned result, an AttributeError is generated (#1724)
        - Update the pylint plugin to latest astroid version (#1708)

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 为 PostgreSQL 和 MySQL 添加 POSIX 正则表达式支持 (#1714)
        - 支持 tortoise.contrib.fastapi.RegisterTortoise 中的 app=None 参数 (#1733)
    
    .. md-tab-item:: 英文

        - Add POSIX Regex support for PostgreSQL and MySQL (#1714)
        - support app=None for tortoise.contrib.fastapi.RegisterTortoise (#1733)


0.21.6
------

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `pydantic_model_creator` 中的错误，当外键未包含在 `include` 参数中时会出错 (#1430)
        - 修复 `contrib.sanic.register_tortoise` 中的错误，导致在使用 asyncpg 和多个工作进程时发生死锁 (#1696)
        - 使用 `.open()` 打开 psycopg 连接池，以消除弃用警告 (#1697)
        - 修复 `bulk_update` 中的错误，当主键字段不是 `id` 时会出错 (#1698)
        - 修复 MySQL UUID 压缩错误 (#1687)
        - 修复 MySQL 中没有约束的外键字段的注释 (#1679)
        - 删除 PostgreSQL 的 no_delay 选项，因为它没有任何作用 (#1677)
        - 修复 `tortoise.models.Model` 中的错误，当 QuerySet 使用 only 函数并且随后使用 print 函数打印返回结果时，会生成 AttributeError (#1723)
    
    .. md-tab-item:: 英文

        - Fix bug in `pydantic_model_creator` when a foreign key is not included in `include` param. (#1430)
        - Fix bug in `contrib.sanic.register_tortoise` causing a deadlock when using asyncpg and > 1 workers (#1696)
        - Open psycopg pool with `.open()` to remove deprecated warning (#1697)
        - Fix bug in `bulk_update` when pk field is not `id` (#1698)
        - Fix mysql uuid compression bug (#1687)
        - Fix comment for fk fields without constraint for mysql (#1679)
        - Removed no_delay option for postgres, as it wasn't doing anything (#1677)
        - Fix bug in `tortoise.models.Model` When a QuerySet uses the only function and then uses the print function to print the returned result, an AttributeError is generated. (#1723)

`0.21.5 <../0.21.5>`_ - 2024-07-18
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 将 `_create_db` 参数传播到 RegisterTortoise 中。 (#1676)
    
    .. md-tab-item:: 英文

        - Propagate `_create_db` parameter to RegisterTortoise. (#1676)

`0.21.4 <../0.21.4>`_ - 2024-07-03
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加 ObjectDoesNotExistError 以显示更好的 404 消息。 (#759)
        - DoesNotExist 和 MultipleObjectsReturned 支持 'Type[Model]' 参数。 (#742)(#1650)
        - 为 RegisterTortoise 添加 use_tz 和 timezone 参数。 (#1649)
        - 支持 await `tortoise.contrib.fastapi.RegisterTortoise`。 (#1662)
        - 添加 `tortoise.contrib.test.init_memory_sqlite`。 (#1657)
    
    .. md-tab-item:: 英文

        - Add ObjectDoesNotExistError to show better 404 message. (#759)
        - DoesNotExist and MultipleObjectsReturned support 'Type[Model]' argument. (#742)(#1650)
        - Add argument use_tz and timezone to RegisterTortoise. (#1649)
        - Support await `tortoise.contrib.fastapi.RegisterTortoise`. (#1662)
        - Add `tortoise.contrib.test.init_memory_sqlite`. (#1657)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `update_or_create` 在字段值更改时的错误。 (#1584)
        - 修复 bandit 检查错误。 (#1643)
        - 修复 ConnectionWrapper 中的潜在竞争条件。 (#1656)
        - 修复 py312 关于 datetime.utcnow 的警告。 (#1661)
        - 修复重用值和 value_list 查询。 (#780)
    
    .. md-tab-item:: 英文

        - Fix `update_or_create` errors when field value changed. (#1584)
        - Fix bandit check error (#1643)
        - Fix potential race condition in ConnectionWrapper (#1656)
        - Fix py312 warning for datetime.utcnow (#1661)
        - Fix reusing values and value_list queries (#780)

Changed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 移除 contrib/test 中过时的 loop._selector。 (#659)(#1636)
    
    .. md-tab-item:: 英文

        - Remove obsolete loop._selector from contrib/test. (#659)(#1636)

`0.21.3 <../0.21.3>`_ - 2024-06-01
------

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复在使用 source_field 作为主键时的 `bulk_update` 问题。 (#1633)
    
    .. md-tab-item:: 英文

        - Fix `bulk_update` when using source_field for pk (#1633)

`0.21.2 <../0.21.2>`_ - 2024-05-25
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 为 M2M 字段添加 `create_unique_index` 参数，并将默认值设为 true (#1620)
    
    .. md-tab-item:: 英文

        - Add `create_unique_index` argument to M2M field and default if it is true (#1620)

`0.21.1 <../0.21.1>`_ - 2024-05-24
------

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复使用旧式 `pk=True` 时的错误。
    
    .. md-tab-item:: 英文

        - Fix error on using old style `pk=True`

`0.21.0 <../0.21.0>`_ - 2024-05-23
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 增强 FastAPI 生命周期支持 (#1371)  
        - 为 Q 添加 `__eq__` 方法，以更轻松地测试动态构建的查询 (#1506)  
        - 为 PostgreSQL 添加 `PlainToTsQuery` 函数 (#1347)  
        - 允许字段的默认关键字为异步函数 (#1498)  
        - 添加对查询集切片的支持 (#1341)  
    
    .. md-tab-item:: 英文

        - Enhancement for FastAPI lifespan support (#1371)
        - Add __eq__ method to Q to more easily test dynamically-built queries (#1506)
        - Added PlainToTsQuery function for postgres (#1347)
        - Allow field's default keyword to be async function (#1498)
        - Add support for queryset slicing. (#1341)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `DatetimeField` 使用 `'__year'` 报告 `'int' object has no attribute 'utcoffset'` 的问题 (#1575)  
        - 修复使用自定义字段时的 `bulk_update` 问题 (#1564)  
        - 修复 `pydantic_model_creator` 中的 `optional` 参数在 pydantic v2 中不工作的情况 (#1551)  
        - 修复 `get_annotations` 现在在默认作用域中评估注解，而不是在应用程序命名空间中 (#1552)  
        - 修复 `get_or_create` 方法 (#1404)  
        - 使用 `index_name` 代替 `BaseSchemaGenerator._generate_index_name` 来生成索引名称。  
        - 在 `QuerySet` 中使用子查询进行 `count()` 和 `exists()` 以匹配计数结果与 `QuerySet` 结果 (#1607)  
    
    .. md-tab-item:: 英文

        - Fix `DatetimeField` use '__year' report `'int' object has no attribute 'utcoffset'`. (#1575)
        - Fix `bulk_update` when using custom fields. (#1564)
        - Fix `optional` parameter in `pydantic_model_creator` does not work for pydantic v2. (#1551)
        - Fix `get_annotations` now evaluates annotations in the default scope instead of the app namespace. (#1552)
        - Fix `get_or_create` method. (#1404)
        - Use `index_name` instead of `BaseSchemaGenerator._generate_index_name` to generate index name.
        - Use subquery for count() and exists() in `QuerySet` to match count result to `QuerySet` result. (#1607)

Changed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 将 `utils.chunk` 从函数更改为懒惰返回可迭代对象。  
        - 移除了生成的 pydantic 模型中 id 键的下限。 (#1602)  
        - 将字段的初始参数 `pk`/`index` 重命名为 `primary_key`/`db_index`。 (#1621)  
        - 将 `Model.check` 方法重命名为 `Model._check` 以避免命名冲突问题。 (#1559) (#1550)  
    
    .. md-tab-item:: 英文

        - Change `utils.chunk` from function to return iterables lazily.
        - Removed lower bound of id keys in generated pydantic models. (#1602)
        - Rename Field initial arguments `pk`/`index` to `primary_key`/`db_index`. (#1621)
        - Renamed `Model.check` method to `Model._check` to avoid naming collision issues  (#1559) (#1550)

Breaking Changes
^^^^^^^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - `bulk_create` 现在不返回任何东西. (#1614)
    
    .. md-tab-item:: 英文

        - `bulk_create` now does not return anything. (#1614)

0.20
====

0.20.1
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 为 `MySQL` 中的 `UUIDField` 添加二进制压缩支持。 (#1458)  
        - 现在只有 `Model`、 `Tortoise`、 `BaseDBAsyncClient`、`__version__` 和 `connections` 从 `tortoise` 导出。  
        - 为 `pydantic_model_creator` 添加参数 `validators`。 (#1471)  
    
    .. md-tab-item:: 英文

        - Add binary compression support for `UUIDField` in `MySQL`. (#1458)
        - Only `Model`, `Tortoise`, `BaseDBAsyncClient`, `__version__`, and `connections` are now exported from `tortoise`
        - Add parameter `validators` to `pydantic_model_creator`. (#1471)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `ValuesListQuery` 中字段顺序，当字段超过 10 个时。 (#1492)  
        - 修复 pydantic v2 中 `pydantic_model_creator` 的可空字段未标记为可选。 (#1454)  
        - 修复 pydantic v2.5 单元测试错误。 (#1535)  
        - 修复 `pydantic_model_creator` 中 `exclude_readonly` 参数无效的问题。  
        - 修复非过滤查询的注释传播问题。 (#1590)  
        - 修复 blacksheep 示例中的单元测试错误。 (#1534)  
    
    .. md-tab-item:: 英文
      
        - Fix order of fields in `ValuesListQuery` when it has more than 10 fields. (#1492)
        - Fix pydantic v2 pydantic_model_creator nullable field not optional. (#1454)
        - Fix pydantic v2.5 unittest error. (#1535)
        - Fix pydantic_model_creator `exclude_readonly` parameter not working.
        - Fix annotation propagation for non-filter queries. (#1590)
        - Fix blacksheep example unittest error. (#1534)

0.20.0
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 允许 `ForeignKeyField(on_delete=NO_ACTION)`。 (#1393)  
        - 支持 `pydantic` 2.0。 (#1433)  
    
    .. md-tab-item:: 英文

        - Allow ForeignKeyField(on_delete=NO_ACTION) (#1393)
        - Support `pydantic` 2.0. (#1433)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 MSSQL Server 上未生成外键约束的问题。 (#1400)  
        - 修复 Python 3.11 的测试用例错误。 (#1308)  
    
    .. md-tab-item:: 英文

        - Fix foreign key constraint not generated on MSSQL Server. (#1400)
        - Fix testcase error with python3.11 (#1308)

Breaking Changes
^^^^^^^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 删除对 `pydantic` 1.x 的支持。  
        - 删除对 `python` 3.7 的支持。  
        - `pydantic_model_creator` 的参数 `config_class` 重命名为 `model_config`。  
        - `PydanticMeta` 的属性 `config_class` 重命名为 `model_config`。  
    
    .. md-tab-item:: 英文

        - Drop support for `pydantic` 1.x.
        - Drop support for `python` 3.7.
        - Param `config_class` of `pydantic_model_creator` is renamed to `model_config`.
        - Attr `config_class` of `PydanticMeta` is renamed to `model_config`.

0.19
====

0.19.3
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加了 `config_class` 选项到 Pydantic 模型生成器，允许开发者自定义生成的 Pydantic 模型的 `Config` 类。 (#1048)
    
    .. md-tab-item:: 英文

        - Added config_class option to pydantic model genator that allows the developer to customize the generated pydantic model's `Config` class. (#1048)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - FastAPI 示例测试无法正常工作。 (#1029)
        - 修复创建索引的 SQL 错误。 (#1202)
        - 修复依赖解析错误。 (#1246)
        - 修复忽略限制的零值。 (#1270)
        - 修复当外键为整数 0 时，`ForeignKeyField` 为 None 的问题。 (#1274)
        - 修复限制忽略零的问题。 (#1270)
        - 修复十进制字段的最小/最大值验证器。 (#1291)
    
    .. md-tab-item:: 英文

        - Fastapi example test not working. (#1029)
        - Fix create index sql error. (#1202)
        - Fix dependencies resolve error. (#1246)
        - Fix ignoring zero value of limit. (#1270)
        - Fix ForeignKeyField is none when fk is integer 0. (#1274)
        - Fix limit ignore zero. (#1270)
        - Fix min/max value validators for decimal fields. (#1291)

0.19.2
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 向模型的 Meta 添加了 `schema` 属性，以指定与模型使用的确切架构。
    
    .. md-tab-item:: 英文

        - Added `schema` attribute to Model's Meta to specify exact schema to use with the model.

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 混合类不工作。（#1133）
        - `using_db` 在模型快捷方法中的位置错误。（#1150）
        - 通过在连接字符串中添加数据库信息修复与 `Oracle` 数据库的连接。
        - 修复在使用 `Oracle` 数据库时出现的 ORA-01435 错误。（#1155）
        - 修复 MySQL 连接字符串中 `ssl` 选项的处理。
        - 修复 `QuerySetSingle` 的类型提示。
    
    .. md-tab-item:: 英文

        - Mixin does not work. (#1133)
        - `using_db` wrong position in model shortcut methods. (#1150)
        - Fixed connection to `Oracle` database by adding database info to DBQ in connection string.
        - Fixed ORA-01435 error while using `Oracle` database (#1155)
        - Fixed processing of `ssl` option in MySQL connection string.
        - Fixed type hinting for `QuerySetSingle`.

0.19.1
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加了 `Postgres`/`SQLite` 部分索引支持。（#1103）
        - 添加了对 `Microsoft SQL Server`/`Oracle` 的支持，基于 `asyncodbc <https://github.com/tortoise/asyncodbc>`_，请注意这**未完全测试**。
        - 为 `pydantic_model_creator` 添加了 `optional` 参数。（#770）
        - 为 `Model` 快捷方法添加了 `using_db` 参数。（#1109）
    
    .. md-tab-item:: 英文

        - Added `Postgres`/`SQLite` partial indexes support. (#1103)
        - Added `Microsoft SQL Server`/`Oracle` support, powered by `asyncodbc <https://github.com/tortoise/asyncodbc>`_, note that which is **not fully tested**.
        - Added `optional` parameter to `pydantic_model_creator`. (#770)
        - Added `using_db` parameter to `Model` shortcut methods. (#1109)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - `MySQL` 的 `TimeField` 将返回 `datetime.timedelta` 对象，而不是 `datetime.time` 对象。
        - 修复冲突时不执行任何操作的问题。（#1122）
        - 修复 `Model._init_from_db` 方法中 `_custom_generated_pk` 属性未设置的问题。（#633）
    
    .. md-tab-item:: 英文

        - `TimeField` for `MySQL` will return `datetime.timedelta` object instead of `datetime.time` object.
        - Fix on conflict do nothing. (#1122)
        - Fix `_custom_generated_pk` attribute not set in `Model._init_from_db` method. (#633)

0.19.0
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加了 `psycopg` 后端支持。
        - 添加了新的统一且强大的连接管理接口，用于访问数据库连接，包括懒加载连接创建等更多功能。有关更多详细信息，请查看此 `PR <https://github.com/tortoise/tortoise-orm/pull/1001>`_。
        - 添加了 `TimeField`。（#1054）
        - 添加了 `ArrayField`。
    
    .. md-tab-item:: 英文

        - Added psycopg backend support.
        - Added a new unified and robust connection management interface to access DB connections which includes support for
        lazy connection creation and much more. For more details, check out this `PR <https://github.com/tortoise/tortoise-orm/pull/1001>`_
        - Added `TimeField`. (#1054)
        - Added `ArrayField`.

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `bulk_create` 在更新多个 `update_fields` 时工作不正常的问题。（#1046）
        - 修复在 PostgreSQL 上为小整数列设置为 null 时 `bulk_update` 出现的错误。（#1086）
    
    .. md-tab-item:: 英文

        - Fix `bulk_create` doesn't work correctly with more than 1 update_fields. (#1046)
        - Fix `bulk_update` errors when setting null for a smallint column on postgres. (#1086)

Deprecated
^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 现有的连接管理接口和相关的公共 API 已被弃用：
        - `Tortoise.get_connection`
        - `Tortoise.close_connections`
    
    .. md-tab-item:: 英文

        - Existing connection management interface and related public APIs which are deprecated:
        - `Tortoise.get_connection`
        - `Tortoise.close_connections`

Changed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 重构 `tortoise.transactions.get_connection` 方法为 `tortoise.transactions._get_connection`.
        注意，该方法现在被标记为 **模块私有，不属于公共 API**。
    
    .. md-tab-item:: 英文

        - Refactored `tortoise.transactions.get_connection` method to `tortoise.transactions._get_connection`.
        Note that this method has now been marked **private to this module and is not part of the public API**

0.18.1
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加冲突以更新批量创建 (bulk_create)。(#1024)
    
    .. md-tab-item:: 英文

        - Add on conflict do update for bulk_create. (#1024)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `bulk_create` 错误。(#1012)
        - 修复 unittest 无效。
        - 修复 `postgres` 中某些类型的 `bulk_update`。(#968) (#1022)
    
    .. md-tab-item:: 英文

        - Fix `bulk_create` error. (#1012)
        - Fix unittest invalid.
        - Fix `bulk_update` in `postgres` with some type. (#968) (#1022)

0.18.0
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加 Case-When 支持。（#943）
        - 在 contrib 中添加 `Rand`/`Random` 函数。（#944）
        - 在 `INSERT` 语句中添加 `ON CONFLICT` 支持。（#428）
    
    .. md-tab-item:: 英文

        - Add Case-When support. (#943)
        - Add `Rand`/`Random` function in contrib. (#944)
        - Add `ON CONFLICT` support in `INSERT` statements. (#428)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 pk 为 uuid 时出现的 `bulk_update` 错误。(#986)
        - 修复可变默认值。(#969)
    
    .. md-tab-item:: 英文

        - Fix `bulk_update` error when pk is uuid. (#986)
        - Fix mutable default value. (#969)

Changed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 将“Function”、“Aggregate”从“functions.py”移至“expressions.py”。(#943)
        - 将“Q”从“query_utils.py”移至“expressions.py”。
        - 将“python-rapidjson”替换为“orjson”。
    
    .. md-tab-item:: 英文

        - Move `Function`, `Aggregate` from `functions.py` to `expressions.py`. (#943)
        - Move `Q` from `query_utils.py` to `expressions.py`.
        - Replace `python-rapidjson` to `orjson`.

Removed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 删除“asynctest”并使用“unittest.IsolatedAsyncioTestCase”。(#416)
        - 删除测试中的“py37”支持。
        - 删除“green”和“nose2”测试运行器。
    
    .. md-tab-item:: 英文

        - Remove `asynctest` and use `unittest.IsolatedAsyncioTestCase`. (#416)
        - Remove `py37` support in tests.
        - Remove `green` and `nose2` test runner.

0.17
====

0.17.8
------

Added
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加 `Model.raw` 方法以支持原始 SQL 查询。
        - 添加 `QuerySet.bulk_update` 方法。（#924）
        - 添加 `QuerySet.in_bulk` 方法。
        - 添加 `MaxValueValidator` 和 `MinValueValidator`（#927）
    
    .. md-tab-item:: 英文

        - Add `Model.raw` method to support the raw sql query.
        - Add `QuerySet.bulk_update` method. (#924)
        - Add `QuerySet.in_bulk` method.
        - Add `MaxValueValidator` and `MinValueValidator` (#927)

Fixed
^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复在实例上运行 `_clone` 时丢失 `QuerySet` 子类的问题。
        - 修复 `.values` 和 `source_field` 中的错误。(#844)
        - 修复 `contrib.blacksheep` 异常处理程序，使用内置 json 响应。(#914)
        - 修复 Meta 类中定义的索引未在其模板中使用 `exists` 参数的问题 (#928)
    
    .. md-tab-item:: 英文

        - Fix `QuerySet` subclass being lost when `_clone` is run on the instance.
        - Fix bug in `.values` with `source_field`. (#844)
        - Fix `contrib.blacksheep` exception handlers, use builtin json response. (#914)
        - Fix Indexes defined in Meta class do not make use of `exists` parameter in their template (#928)

Changed
^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 允许使用 `IntEnumField` 的负值。(#889)
        - 使 `.values()` 和 `.values_list()` 等待的返回值更加一致。(#899)
    
    .. md-tab-item:: 英文

        - Allow negative values with `IntEnumField`. (#889)
        - Make `.values()` and `.values_list()` awaited return more consistent. (#899)

0.17.7
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复正向关系的 `select_related` 行为。（#825）
        - 修复嵌套 `QuerySet` 和 `Manager` 中的错误。（#864）
        - 为 MySQL/PostgreSQL 添加 `Concat` 函数。（#873）
        - 修补查询时 use_index/force_index 可变问题。（#888）
        - 提升查询中的注释字段优先级。（#883）
        - 在选择类型查询中提供 use/force index。（#893）
        - 修复所有日志记录以使用 Tortoise 的记录器而不是根记录器。（#879）
        - 将 `db_client` 记录器重命名为 `tortoise.db_client`。
        - 将 `indexes` 添加到 `Model.describe`。
    
    .. md-tab-item:: 英文

        - Fix `select_related` behaviour for forward relation. (#825)
        - Fix bug in nested `QuerySet` and `Manager`. (#864)
        - Add `Concat` function for MySQL/PostgreSQL. (#873)
        - Patch for use_index/force_index mutable problem when making query. (#888)
        - Lift annotation field's priority in make query. (#883)
        - Make use/force index available in select type Query. (#893)
        - Fix all logging to use Tortoise's logger instead of root logger. (#879)
        - Rename `db_client` logger to `tortoise.db_client`.
        - Add `indexes` to `Model.describe`.

0.17.6
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加 `RawSQL` 表达式。
        - 修复 `_make_query` 中带有注释的列计数问题。(#776)
        - 使函数嵌套。(#828)
        - 在字段描述中添加 `db_constraint`。
    
    .. md-tab-item:: 英文

        - Add `RawSQL` expression.
        - Fix columns count with annotations in `_make_query`. (#776)
        - Make functions nested. (#828)
        - Add `db_constraint` in field describe.

0.17.5
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 将 fk 和 o2o 的 `field_type` 设置为与关系字段类型相同。（#443）
        - 修复 `.sql()` 调用多次时的错误 sql。（#796）
        - 修复使用 Router 时导入路由的不正确拆分（#798）
        - 修复使用 `F` 进行 `annotate` 后的 `filter` 错误。（#806）
        - 修复反向关系的 `select_related`。（#808）
    
    .. md-tab-item:: 英文

        - Set `field_type` of fk and o2o same to which relation field type. (#443)
        - Fix error sql for `.sql()` call more than once. (#796)
        - Fix incorrect splitting of the import route when using Router (#798)
        - Fix `filter` error after `annotate` with `F`. (#806)
        - Fix `select_related` for reverse relation. (#808)

0.17.4
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 `update_or_create`。（#782）
        - 将 `contains`、`contained_by` 和 `filter` 添加到 `JSONField`
    
    .. md-tab-item:: 英文

        - Fix `update_or_create`. (#782)
        - Add `contains`, `contained_by` and `filter` to `JSONField`

0.17.3
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复在 M2M 关系上使用自定义关联类时出现的重复问题
        - 修复 `update_or_create` 和 `get_or_create`。（#721）
        - 修复 `refresh_from_db` 未传递字段的问题。（#734）
        - 使 `update` 查询与 `limit` 和 `order_by` 配合使用。（#748）
        - 添加 `Subquery` 表达式。（#756）（#9）（#337）
        - 在 JSONField 中使用 JSON。
    
    .. md-tab-item:: 英文

        - Fix duplicates when using custom through association class on M2M relations
        - Fix `update_or_create` and `get_or_create`. (#721)
        - Fix `refresh_from_db` without fields pass. (#734)
        - Make `update` query work with `limit` and `order_by`. (#748)
        - Add `Subquery` expression. (#756) (#9) (#337)
        - Use JSON in JSONField.

0.17.2
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加更多 `index` 类型。
        - 为 `queryset` 添加 `force_index`、`use_index`。
        - 使用 `update_fields` 修复更新错误中的 `F`。
        - 使 `delete` 查询与 `limit` 和 `order_by` 配合使用。(#697)
        - 使用 `IS NULL` 和 `NOT IS NULL` 过滤器过滤反向 FK 字段 (#700)
        - 在 `update_or_create` 中添加 `select_for_update`。(#702)
        - 添加 `Model.select_for_update`。
        - 为查询集添加 `__search` 全文搜索。
    
    .. md-tab-item:: 英文

        - Add more `index` types.
        - Add `force_index`, `use_index` to `queryset`.
        - Fix `F` in update error with `update_fields`.
        - Make `delete` query work with `limit` and `order_by`. (#697)
        - Filter backward FK fields with `IS NULL` and `NOT IS NULL` filters (#700)
        - Add `select_for_update` in `update_or_create`. (#702)
        - Add `Model.select_for_update`.
        - Add `__search` full text search to queryset.

0.17.1
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复模块类型。
        - 修复多次指定相关模型时出现的 `select_related` 问题。（#679）
        - 向模型添加 `__iter__`，现在可以在 `fastapi` 响应中返回模型/模型。
        - 修复由 'router' 引起的 `in_transaction` 错误。（#677）（#678）
    
    .. md-tab-item:: 英文

        - Fix type for modules.
        - Fix `select_related` when related model specified more than once. (#679)
        - Add `__iter__` to model, now can just return model/models in `fastapi` response.
        - Fix `in_transaction` bug caused by 'router'. (#677) (#678)

0.17.0
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加日期部分提取过滤。
        - 添加 `Manager` 支持。
        - 添加 db router 支持。
        - 为 `queryset.select_for_update` 添加 `nowait`、`skip_locked`、`of` 参数。
        - 为验证异常添加字段名称。
        - 与 `asyncmy <https://github.com/long2ice/asyncmy>` 兼容。
        - 将 pypika 替换为 `pypika-tortoise <https://github.com/tortoise/pypika-tortoise>`。
    
    .. md-tab-item:: 英文

        - Add date part extract filtering.
        - Add `Manager` support.
        - Add db router support.
        - Add `nowait`, `skip_locked`, `of` parameters to `queryset.select_for_update`.
        - Add field name to validation exceptions.
        - Compatible with `asyncmy <https://github.com/long2ice/asyncmy>`_.
        - Replace pypika to `pypika-tortoise <https://github.com/tortoise/pypika-tortoise>`_.

0.16
====

0.16.21
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复了解码前验证 JSON 的问题。（#623）
        - 添加模型方法 `update_or_create`。
        - 为 `bulk_create` 方法添加 `batch_size` 参数。
        - 修复使用 F 表达式保存和使用 source_field 字段的问题。
    
    .. md-tab-item:: 英文

        - Fixed validating JSON before decoding. (#623)
        - Add model method `update_or_create`.
        - Add `batch_size` parameter for `bulk_create` method.
        - Fix save with F expression and field with source_field.

0.16.20
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 添加模型字段验证器。
        - 允许函数结果按组分组。（#608）
    
    .. md-tab-item:: 英文

        - Add model field validators.
        - Allow function results in group by. (#608)

0.16.19
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 将“TZ”环境变量设置为“TIMEZONE”，以避免影响全局时区。
        - 允许将模块对象传递给“Tortoise.init_models()”的“models_paths”参数。（#561）
        - 实现“PydanticMeta.backward_relations”。（#536）
        - 允许在“PydanticModelCreator”中覆盖“PydanticMeta”。（#536）
        - 修复了时区模块中 make_native 拼写错误为 make_naive
    
    .. md-tab-item:: 英文

        - Replace set `TZ` environment variable to `TIMEZONE` to avoid affecting global timezone.
        - Allow passing module objects to `models_paths` param of `Tortoise.init_models()`. (#561)
        - Implement `PydanticMeta.backward_relations`. (#536)
        - Allow overriding `PydanticMeta` in `PydanticModelCreator`. (#536)
        - Fixed make_native typo to make_naive in timezone module

0.16.18
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 支持更新中的自定义函数。（#537）
        - 添加 `Model.refresh_from_db`。（#549）
        - 添加时区支持，**升级到此版本时请谨慎**，详情请参阅 `docs <https://tortoise-orm.readthedocs.io/en/latest/timezone.html>`_。（#335）
        - 删除 `aerich` 以防循环依赖。（#558）
    
    .. md-tab-item:: 英文

        - Support custom function in update. (#537)
        - Add `Model.refresh_from_db`. (#549)
        - Add timezone support, **be careful to upgrade to this version**, see `docs <https://tortoise-orm.readthedocs.io/en/latest/timezone.html>`_ for details. (#335)
        - Remove `aerich` in case of cyclic dependency. (#558)

0.16.17
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 在 `ManyToManyField` 中添加 `on_delete`。（#508）
        - 在 `annotate` 中支持 `F` 表达式。（#475）
        - 修复 `QuerySet.select_related`，以防两次连接同一张表。（#525）
        - 将 Aerich 集成到安装中。（#530）
    
    .. md-tab-item:: 英文

        - Add `on_delete` in `ManyToManyField`. (#508)
        - Support `F` expression in `annotate`. (#475)
        - Fix `QuerySet.select_related` in case of join same table twice. (#525)
        - Integrate Aerich into the install. (#530)

0.16.16
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复 FastAPI 完整性错误异常不一致问题
        - 将 OSError 添加到 _get_comments except 块
    
    .. md-tab-item:: 英文

        - Fixed inconsistency in integrity error exception of FastAPI
        - add OSError to _get_comments except block

0.16.15
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 使 `DateField` 接受有效的日期字符串。
        - 添加 `QuerySet.select_for_update()`。
        - 在创建 pydantic 模型时检查 ``default`` 是否为 ``None``
        - 将默认值传播到 pydantic 模型
        - 添加 `QuerySet.select_related()`。
        - 为 Prefetch 指令添加自定义属性名称。
        - 为 `RelationalField` 系列添加 `db_constraint`。
    
    .. md-tab-item:: 英文

        - Make `DateField` accept valid date str.
        - Add `QuerySet.select_for_update()`.
        - check ``default`` for not ``None`` on pydantic model creation
        - propagate default to pydantic model
        - Add `QuerySet.select_related()`.
        - Add custom attribute name for Prefetch instruction.
        - Add `db_constraint` for `RelationalField` family.

0.16.14
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 使“F”表达式与“QuerySet.filter()”一起工作。
        - 在源分发中包含“py.typed”。
        - 为“fields.DatetimeField”添加了从“int”解析“datetime”的功能。
        - 如果提供，“get_or_create”将传递“using_db=”。
        - 允许将自定义“loop”和“connection_class”参数传递给asyncpg。
    
    .. md-tab-item:: 英文

        - Make ``F`` expression work with ``QuerySet.filter()``.
        - Include ``py.typed`` in source distribution.
        - Added ``datetime`` parsing from ``int`` for ``fields.DatetimeField``.
        - ``get_or_create`` passes the ``using_db=`` on if provided.
        - Allow custom ``loop`` and ``connection_class`` parameters to be passed on to asyncpg.

0.16.13
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - ``tortoise-orm`` 的默认安装现在不包含任何 C 依赖项，如果您想使用 C 加速器，请改为执行 ``pip install tortoise-orm[accel]``。
        - 添加了 ``<instance>.clone()`` 方法，该方法将在内存中创建一个克隆实例。要保留它，您仍然需要调用 ``.save()``
        - 如果 tortoise 无法生成主键，``.clone()`` 将引发 ``ParamsError``。在这种情况下，请执行 ``.clone(pk=<newval>)``
        - 如果手动将主键值设置为 ``None`` 并且可以自动生成主键，这将创建一个新记录。但是，我们仍然建议改用 ``.clone()`` 方法。
        - 可以通过设置“force_create=True”强制“.save()”执行创建操作
        - 可以通过设置“force_update=True”强制“.save()”执行更新操作
        - 为“.save()”操作设置“update_fields”将强烈建议尽可能执行更新操作
    
    .. md-tab-item:: 英文

        - Default install of ``tortoise-orm`` now installs with no C-dependencies, if you want to use the C accelerators, please do a ``pip install tortoise-orm[accel]`` instead.
        - Added ``<instance>.clone()`` method that will create a cloned instance in memory. To persist it you still need to call ``.save()``
        - ``.clone()`` will raise a ``ParamsError`` if tortoise can't generate a primary key. In that case do a ``.clone(pk=<newval>)``
        - If manually setting the primary key value to ``None`` and the primary key can be automatically generated, this will create a new record. We however still recommend the ``.clone()`` method instead.
        - ``.save()`` can be forced to do a create by setting ``force_create=True``
        - ``.save()`` can be forced to do an update by setting ``force_update=True``
        - Setting ``update_fields`` for a ``.save()`` operation will strongly prefer to do an update if possible

0.16.12
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 生成表时使 ``Field.default`` 在数据库级别生效
        - 添加转换器，而不是从 pymysql 导入
        - 修复 postgres BooleanField 默认值转换
        - 修复 ``pydantic_model_creator`` 中输入的 ``JSONField``
        - 在 ``QuerySet`` 上添加 ``.sql()`` 方法
    
    .. md-tab-item:: 英文

        - Make ``Field.default`` effect on db level when generate table
        - Add converters instead of importing from pymysql
        - Fix postgres BooleanField default value convent
        - Fix ``JSONField`` typed in ``pydantic_model_creator``
        - Add ``.sql()`` method on ``QuerySet``

0.16.11
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复：Windows 中的 ``sqlite://:memory:`` 抛出 ``OSError: [WinError 123]``
        - 当主键由 DB 生成时，支持 ``bulk_create()`` 插入具有覆盖主键的记录
        - 添加 ``queryset.exists()`` 和 ``Model.exists()``。
        - 添加模型订阅查找，``Model[<pkval>]``，它将返回对象或引发 ``KeyError``
    
    .. md-tab-item:: 英文

        - fix: ``sqlite://:memory:`` in Windows thrown ``OSError: [WinError 123]``
        - Support ``bulk_create()`` insertion of records with overridden primary key when the primary key is DB-generated
        - Add ``queryset.exists()`` and ``Model.exists()``.
        - Add model subscription lookup, ``Model[<pkval>]`` that will return the object or raise ``KeyError``

0.16.10
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复“basestring”的错误导入
        - 更好地处理字符串中的 NULL 字符。修复 SQLite，为 PostgreSQL 引发更好的错误。
        - 现在支持带有 join 的“.group_by()”
    
    .. md-tab-item:: 英文

        - Fix bad import of ``basestring``
        - Better handling of NULL characters in strings. Fixes SQLite, raises better error for PostgreSQL.
        - Support ``.group_by()`` with join now

0.16.9
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 现在支持 ``.save()`` 中的 ``F`` 表达式
        - ``IntEnumField`` 接受有效的 int 值，``CharEnumField`` 接受有效的 str 值
        - 使用全局唯一标识符创建 Pydantic 模型
        - 叶子检测以尽量减少重复的 Pydantic 模型创建
        - 当 ``exclude_raw_fields=True`` 时，具有主键（也是关系的原始字段）的 Pydantic 模型现在不会隐藏，因为它是一个至关重要的字段
        - 当字段同时设置为可空和主键时引发信息性错误
        - 现在描述外键 ID 具有与其相关的字段的正整数范围
        - 修复了 OneToOne 关系的预取
        - 修复了非文本字段的 ``__contains``（例如 ``JSONB``）
    
    .. md-tab-item:: 英文

        - Support ``F`` expression in ``.save()`` now
        - ``IntEnumField`` accept valid int value and ``CharEnumField`` accept valid str value
        - Pydantic models get created with globally unique identifier
        - Leaf-detection to minimize duplicate Pydantic model creation
        - Pydantic models with a Primary Key that is also a raw field of a relation is now not hidden when ``exclude_raw_fields=True`` as it is a critically important field
        - Raise an informative error when a field is set as nullable and primary key at the same time
        - Foreign key id's are now described to have the positive-integer range of the field it is related to
        - Fixed prefetching over OneToOne relations
        - Fixed ``__contains`` for non-text fields (e.g. ``JSONB``)

0.16.8
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 允许 ``Q`` 表达式与 ``_filter`` 参数一起在聚合上起作用
        - 添加手动 ``.group_by()`` 支持
        - 修复了在具有指定顺序的聚合中缺少 ``GROUP BY`` 类的回归问题。
    
    .. md-tab-item:: 英文

        - Allow ``Q`` expression to function with ``_filter`` parameter on aggregations
        - Add manual ``.group_by()`` support
        - Fixed regression where ``GROUP BY`` class is missing for an aggregate with a specified order.

0.16.7
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 增加了对 Python 3.9 的初步支持
        - ``TruncationTestCase`` 现在在清除表名时可以正确引用表名。
        - 添加模型信号支持
        - 将 ``app_label`` 添加到 ``test initializer(...)`` 并将 ``TORTOISE_TEST_APP`` 作为测试环境变量。
    
    .. md-tab-item:: 英文

        - Added preliminary support for Python 3.9
        - ``TruncationTestCase`` now properly quotes table names when it clears them out.
        - Add model signals support
        - Added ``app_label`` to ``test initializer(...)`` and ``TORTOISE_TEST_APP`` as test environment variable.

0.16.6
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. warning::

            这是一个安全修复版本。我们建议每个人都进行更新。
    
    .. md-tab-item:: 英文

        .. warning::

            This is a security fix release. We recommend everyone update.

Security fixes
^^^^^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复了 MySQL 中的 SQL 注入问题
        - 修复了使用“contains”、“starts_with”或“ends_with”过滤器（及其不区分大小写的对应项）时 MySQL 中的 SQL 注入问题
        - 修复了使用“contains”、“starts_with”或“ends_with”过滤器（及其不区分大小写的对应项）时 PostgreSQL 和 SQLite 的格式错误的 SQL
    
    .. md-tab-item:: 英文

        - Fixed SQL injection issue in MySQL
        - Fixed SQL injection issues in MySQL when using ``contains``, ``starts_with`` or ``ends_with`` filters (and their case-insensitive counterparts)
        - Fixed malformed SQL for PostgreSQL and SQLite when using ``contains``, ``starts_with`` or ``ends_with`` filters (and their case-insensitive counterparts)

Other changes
^^^^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 增加了对部分模型的支持：

        要创建部分模型，可以执行 ``.only(<fieldnames-as-strings>)`` 作为 QuerySet 的一部分。

        这将创建仅包含这些值的模型实例。

        仅当满足以下条件时才允许对模型进行更改：

        * 您要更新的所有字段均在 ``<model>.save(update_fields=[...])`` 中指定
        * 您在 ``.only(...)`` 中包含了模型主键

        为了防止出现常见错误，我们确保会引发错误：

        * 如果您访问未指定的字段，您将收到 ``AttributeError``。
        * 如果您执行 ``<model>.save()``，则会引发 ``IncompleteInstanceError``，因为模型不完整，正如所要求的那样。
        * 如果您执行 ``<model>.save(update_fields=[...])`` 并且您没有在 ``.only(...)`` 中包含主键，
        则将引发 ``IncompleteInstanceError``，表示在不知道主键的情况下无法进行更新。
        * 如果您执行 ``<model>.save(update_fields=[...])`` 并且 ``update_fields`` 中的某个字段不在 ``.only(...)`` 中，
        则将引发 ``IncompleteInstanceError``，因为该字段无法更新。

        - 修复了在外键上执行 ``.values()`` 查询时生成的错误 SQL
        - 添加了 ``<model>.update_from_dict({...})`，它将安全地从字典中批量更新值
        - 修复了在连接字符串中处理 URL 编码密码的问题
    
    .. md-tab-item:: 英文

        * Added support for partial models:

        To create a partial model, one can do a ``.only(<fieldnames-as-strings>)`` as part of the QuerySet.
        This will create model instances that only have those values fetched.

        Persisting changes on the model is allowed only when:

        * All the fields you want to update is specified in ``<model>.save(update_fields=[...])``
        * You included the Model primary key in the ``.only(...)``

        To protect against common mistakes we ensure that errors get raised:

        * If you access a field that is not specified, you will get an ``AttributeError``.
        * If you do a ``<model>.save()`` a ``IncompleteInstanceError`` will be raised as the model is, as requested, incomplete.
        * If you do a ``<model>.save(update_fields=[...])`` and you didn't include the primary key in the ``.only(...)``,
            then ``IncompleteInstanceError`` will be raised indicating that updates can't be done without the primary key being known.
        * If you do a ``<model>.save(update_fields=[...])`` and one of the fields in ``update_fields`` was not in the ``.only(...)``,
            then ``IncompleteInstanceError`` as that field is not available to be updated.

        - Fixed bad SQL generation when doing a ``.values()`` query over a Foreign Key
        - Added `<model>.update_from_dict({...})` that will mass update values safely from a dictionary
        - Fixed processing URL encoded password in connection string

0.16.5
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 将 ``Tortoise.describe_model(<MODEL>, ...)`` 移至 ``<MODEL>.describe(...)``
        * 弃用 ``Tortoise.describe_model()``
        * 修复 ``generate_schemas`` 参数在 ``tortoise.contrib.quart.register_tortoise`` 中被忽略的问题
        * 修复使用 ``source_field`` 参数的连接查询
    
    .. md-tab-item:: 英文

        * Moved ``Tortoise.describe_model(<MODEL>, ...)`` to ``<MODEL>.describe(...)``
        * Deprecated ``Tortoise.describe_model()``
        * Fix for ``generate_schemas`` param being ignored in ``tortoise.contrib.quart.register_tortoise``
        * Fix join query with `source_field` param

0.16.4
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 更一致地转义数据库列，修复使用 SQL 保留关键字作为函数字段名称的问题。
        * 修复在进行自引用聚合时使用连接的错误一侧进行聚合的问题。
        * 修复包装的 ``F`` 函数忘记了 ``distinct=True``
    
    .. md-tab-item:: 英文

        * More consistent escaping of db columns, fixes using SQL reserved keywords as field names with a function.
        * Fix the aggregates using the wrong side of the join when doing a self-referential aggregation.
        * Fix ``F`` functions wrapped forgetting about ``distinct=True``

0.16.3
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 修复使用 ``__in=`` 和 ``__not_in`` 过滤器生成的无效 ``var IN ()`` SQL。
        * 修复嵌套字段上的 order_by 错误
        * 修复通过反向外键与自身连接以进行过滤和注释的问题
    
    .. md-tab-item:: 英文

        * Fixed invalid ``var IN ()`` SQL generated using ``__in=`` and ``__not_in`` filters.
        * Fix bug with order_by on nested fields
        * Fix joining with self by reverse-foreign-key for filtering and annotation

0.16.2
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 默认的 ``values()`` 和 ``values_list()`` 现在包含注释。
        * 连接上的注释现在可以与 ``values()`` 和 ``values_list()`` 正确配合使用
        * 确保 ``GROUP BY`` 位于 ``HAVING`` 之前，以确保按聚合进行筛选可以正常工作。
        * 修复使用聚合的连接查询中的错误
        * 在 SQLite 和 MySQL 上正确转换 ``BooleanField`` 值
    
    .. md-tab-item:: 英文

        * Default ``values()`` & ``values_list()`` now includes annotations.
        * Annotations over joins now work correctly with ``values()`` & ``values_list()``
        * Ensure ``GROUP BY`` precedes ``HAVING`` to ensure that filtering by aggregates work correctly.
        * Fix bug with join query with aggregation
        * Cast ``BooleanField`` values correctly on SQLite & MySQL

0.16.1
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * ``QuerySetSingle`` 现在具有更好的代码完成功能
        * 创建的 Pydantic 模型现在将具有基本的验证元素：

        * 正确填充了必填字段的 ``required``
        * 将 ``nullable`` 添加到接受空值的架构中
        * CharFields 的 ``maxLength``
        * 整数字段的 ``minimum`` 和 ``maximum`` 值

        要让 Pydantic 正确处理可空/默认字段，在将值传递给 Model 类时应执行 ``**user.dict(exclude_unset=True)``。

        * 添加了基于 ``starlette`` 助手的 ``FastAPI`` 助手，但可选择添加助手来捕获并报告正确的错误 ``DoesNotExist`` 和 ``IntegrityError`` Tortoise 异常。
        * 通过在调用“pydantic_model_creator”时设置“exclude_readonly=True”，允许 Pydantic 模型排除所有只读字段。
        * Tortoise“PydanticModel”现在提供两个额外的辅助函数：

        *“from_queryset”：返回“List[PydanticModel]”，这是 FastAPI 期望的格式
        *“from_queryset_single”：允许避免多次调用“await”来获取对象及其所有相关项目。
    
    .. md-tab-item:: 英文

        * ``QuerySetSingle`` now has better code completion
        * Created Pydantic models will now have the basic validation elements:

        * ``required`` is correctly populated for required fields
        * ``nullable`` is added to the schema where nulls are accepted
        * ``maxLength`` for CharFields
        * ``minimum`` & ``maximum`` values for integer fields

        To get Pydantic to handle nullable/default fields correctly one should do a ``**user.dict(exclude_unset=True)`` when passing values to a Model class.

        * Added ``FastAPI`` helper that is based on the ``starlette`` helper but optionally adds helpers to catch and report with proper error ``DoesNotExist`` and ``IntegrityError`` Tortoise exceptions.
        * Allows a Pydantic model to exclude all read-only fields by setting ``exclude_readonly=True`` when calling ``pydantic_model_creator``.
        * a Tortoise ``PydanticModel`` now provides two extra helper functions:

        * ``from_queryset``: Returns a ``List[PydanticModel]`` which is the format that e.g. FastAPI expects
        * ``from_queryset_single``: allows one to avoid calling ``await`` multiple times to get the object and all its related items.


0.16.0
------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. caution::
            
            **此版本不再支持 Python 3.6：**

            Tortoise ORM 现在至少需要 CPython 3.7
    
    .. md-tab-item:: 英文

        .. caution::
            **This release drops support for Python 3.6:**

            Tortoise ORM now requires a minimum of CPython 3.7

New features:
^^^^^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 模型文档字符串和紧随字段定义的 ``#:`` 注释现在被用作文档字符串和 DDL 描述。

            这现在被清理并作为 ``describe_model(...)`` 中的 ``docstring`` 参数的一部分传递。

            如果没有明确指定字段的 ``description=`` 或模型的 ``Meta.table_description=``，则默认为第一行作为描述。
            这样做是因为描述会提交给数据库，且需要短小（根据数据库，最多 63 个字符）。

            使用示例：

            .. code-block:: python3

                class Something(Model):
                    """
                    A Docstring.

                    Some extra info.
                    """

                    # A regular comment
                    name = fields.CharField(max_length=50)
                    #: A docstring comment
                    chars = fields.CharField(max_length=50, description="Some chars")
                    #: A docstring comment
                    #: Some more detail
                    blip = fields.CharField(max_length=50)

                # 查看描述模型时：
                {
                    "description": "A Docstring.",
                    "docstring": "A Docstring.\n\nSome extra info.",
                    ...
                    "data_fields": [
                        {
                            "name": "name",
                            ...
                            "description": null,
                            "docstring": null
                        },
                        {
                            "name": "chars",
                            ...
                            "description": "Some chars",
                            "docstring": "A docstring comment"
                        },
                        {
                            "name": "blip",
                            ...
                            "description": "A docstring comment",
                            "docstring": "A docstring comment\nSome more detail"
                        }
                    ]
                }

        * 模型的早期部分初始化。

            我们现在有了模型的早期初始化，这在需要不绑定到数据库但仍然完整的模型时非常有用。
            例如，在无需正确设置的情况下生成模式。

            使用示例：

            .. code-block:: python3

                # 假设您在 "some/models.py" 和 "other/ddef.py" 中定义了模型
                # 并且您将在 "model" 命名空间中使用它们：
                Tortoise.init_models(["some.models", "other.ddef"], "models")

                # 现在模型将建立关系，因此模式的自省将是全面的

        * Pydantic 序列化。

            我们现在原生支持从 Tortoise ORM 模型自动构建 Pydantic 模型。
            这将正确建模：

            * 数据字段
            * 关系（外键/一对一/多对多）
            * 可调用对象

            目前我们只支持序列化，而不支持反序列化。

            有关更多信息，请参见 :ref:`contrib_pydantic`

        - 允许在注释中使用 ``F`` 表达式。 (#301)
        - 现在负数与 ``limit(...)`` 和 ``offset(...)`` 一起使用时会引发 ``ParamsError``。 (#306)
        - 允许在 ``queryset.update()`` 中使用函数。 (#308)
        - 增加向聚合函数提供 ``distinct`` 标志的能力。 (#312)

    .. md-tab-item:: 英文

        * Model docstrings and ``#:`` comments directly preceding Field definitions are now used as docstrings and DDL descriptions.

            This is now cleaned and carried as part of the ``docstring`` parameter in ``describe_model(...)``

            If one doesn't explicitly specify a Field ``description=`` or Model ``Meta.table_description=`` then we default to the first line as the description.
            This is done because a description is submitted to the DB, and needs to be short (depending on DB, 63 chars) in size.

            Usage example:

            .. code-block:: python3

                class Something(Model):
                    """
                    A Docstring.

                    Some extra info.
                    """

                    # A regular comment
                    name = fields.CharField(max_length=50)
                    #: A docstring comment
                    chars = fields.CharField(max_length=50, description="Some chars")
                    #: A docstring comment
                    #: Some more detail
                    blip = fields.CharField(max_length=50)

                # When looking at the describe model:
                {
                    "description": "A Docstring.",
                    "docstring": "A Docstring.\n\nSome extra info.",
                    ...
                    "data_fields": [
                        {
                            "name": "name",
                            ...
                            "description": null,
                            "docstring": null
                        },
                        {
                            "name": "chars",
                            ...
                            "description": "Some chars",
                            "docstring": "A docstring comment"
                        },
                        {
                            "name": "blip",
                            ...
                            "description": "A docstring comment",
                            "docstring": "A docstring comment\nSome more detail"
                        }
                    ]
                }

        * Early Partial Init of models.

            We now have an early init of models, which can be useful when needing Models that are not bound to a DB, but otherwise complete.
            e.g. Schema generation without needing to be properly set up.

            Usage example:

            .. code-block:: python3

                # Lets say you defined your models in "some/models.py", and "other/ddef.py"
                # And you are going to use them in the "model" namespace:
                Tortoise.init_models(["some.models", "other.ddef"], "models")

                # Now the models will have relationships built, so introspection of schema will be comprehensive

        * Pydantic serialisation.

            We now include native support for automatically building a Pydantic model from Tortoise ORM models.
            This will correctly model:

            * Data Fields
            * Relationships (FK/O2O/M2M)
            * Callables

            At this stage we only support serialisation, not deserialisation.

            For mode information, please see :ref:`contrib_pydantic`

        - Allow usage of ``F`` expressions to in annotations. (#301)
        - Now negative number with ``limit(...)`` and ``offset(...)`` raise ``ParamsError``. (#306)
        - Allow usage of Function to ``queryset.update()``. (#308)
        - Add ability to supply ``distinct`` flag to Aggregate (#312)


Bugfixes:
^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - Fix default type of ``JSONField``
    
    .. md-tab-item:: 英文

        - Fix default type of ``JSONField``

Removals:
^^^^^^^^^

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 删除了“tortoise.aggregation”，因为自 0.14.0 以来它已被弃用
        - 删除了“start_transaction”，因为自 0.15.0 以来它已损坏
        - 删除了对 Python 3.6 / PyPy-3.6 的支持，因为自 0.15.0 以来它已损坏

            如果您仍然需要 Python 3.6 支持，您可以安装“tortoise-orm<0.16”，因为我们仍会在一段时间内将关键错误修复反向移植到 0.15 分支。
    
    .. md-tab-item:: 英文

        - Removed ``tortoise.aggregation`` as this was deprecated since 0.14.0
        - Removed ``start_transaction`` as it has been broken since 0.15.0
        - Removed support for Python 3.6 / PyPy-3.6, as it has been broken since 0.15.0

            If you still need Python 3.6 support, you can install ``tortoise-orm<0.16`` as we will still backport critical bugfixes to the 0.15 branch for a while.

.. rst-class:: emphasize-children

0.15
====

0.15.24
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复了在具有指定顺序的聚合中缺少“GROUP BY”类的回归问题。
    
    .. md-tab-item:: 英文

        - Fixed regression where ``GROUP BY`` class is missing for an aggregate with a specified order.

0.15.23
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 修复了 MySQL 中的 SQL 注入问题
        - 修复了使用“contains”、“starts_with”或“ends_with”过滤器（及其不区分大小写的对应项）时 MySQL 中的 SQL 注入问题
        - 修复了使用“contains”、“starts_with”或“ends_with”过滤器（及其不区分大小写的对应项）时 PostgreSQL 和 SQLite 的格式错误的 SQL
    
    .. md-tab-item:: 英文

        - Fixed SQL injection issue in MySQL
        - Fixed SQL injection issues in MySQL when using ``contains``, ``starts_with`` or ``ends_with`` filters (and their case-insensitive counterparts)
        - Fixed malformed SQL for PostgreSQL and SQLite when using ``contains``, ``starts_with`` or ``ends_with`` filters (and their case-insensitive counterparts)

0.15.22
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 修复在进行自引用聚合时使用错误连接侧的聚合问题。
        * 修复“tortoise.contrib.quart.register_tortoise”中忽略“generate_schemas”参数的问题
    
    .. md-tab-item:: 英文

        * Fix the aggregates using the wrong side of the join when doing a self-referential aggregation.
        * Fix for ``generate_schemas`` param being ignored in ``tortoise.contrib.quart.register_tortoise``

0.15.21
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        * 修复使用 ``__in=`` 和 ``__not_in`` 过滤器生成的无效 ``var IN ()`` SQL。
        * 修复嵌套字段上的 order_by 错误
        * 修复通过反向外键与自身连接以进行过滤和注释的问题
    
    .. md-tab-item:: 英文

        * Fixed invalid ``var IN ()`` SQL generated using ``__in=`` and ``__not_in`` filters.
        * Fix bug with order_by on nested fields
        * Fix joining with self by reverse-foreign-key for filtering and annotation

0.15.20
-------
* Default ``values()`` & ``values_list()`` now includes annotations.
* Annotations over joins now work correctly with ``values()`` & ``values_list()``
* Ensure ``GROUP BY`` precedes ``HAVING`` to ensure that filtering by aggregates work correctly.
* Cast ``BooleanField`` values correctly on SQLite & MySQL

0.15.19
-------
- Fix Function with ``source_field`` option. (#311)

0.15.18
-------
- Install on Windows does not require a C compiler any more.
- Fix ``IntegrityError`` with unique field and ``get_or_create``

0.15.17
-------
- Now ``get_or_none(...)``, classmethod of ``Model`` class, works in the same way as ``queryset``

0.15.16
-------
- ``get_or_none(...)`` now raises ``MultipleObjectsReturned`` if multiple object fetched. (#298)

0.15.15
-------
- Add ability to suppply a ``to_field=`` parameter for FK/O2O to a non-PK but still uniquely indexed remote field. (#287)

0.15.14
-------
- add F expression support in ``queryset.update()`` - This allows for atomic updates of data in the database. (#294)

0.15.13
-------
- Applies default ordering on related queries
- Fix post-ManyToMany related queries not being evaluated correctly
- Ordering is now preserved on ManyToMany related fetches
- Fix aggregate function on joined table to use correct primary key
- Fix filtering by backwards FK to use correct primary key

0.15.12
-------
- Added ``range`` filter to support ``between and`` syntax

0.15.11
-------
- Added ``ordering`` option for model ``Meta`` class to apply default ordering

0.15.10
-------
- Bumped requirements to cater for newer feature use (#282)

0.15.9
------
- Alias Foreign Key joins as we can have both self-referencing and duplicate joins to the same table.
  This generates SQL that differentiates between which instance of the table to work with.

0.15.8
------
- ``TextField`` now recommends usage of ``CharField`` if wanting unique indexing instead of just saying "indexing not supported"
- ``.count()`` now honours offset and limit
- Testing un-awaited ``ForeignKeyField`` as a boolean expression will automatically resolve as ``False`` if it is None
- Awaiting a nullable ``ForeignKeyField`` won't touch the DB if it is ``None``

0.15.7
------
- ``QuerySet.Update()`` now returns the count of the no of rows affected. Note, that
- ``QuerySet.Delete()`` now returns the count of the no of rows deleted.
- Note that internal API of ``db_connection.execute_query()`` now returns ``rows_affected, results``. (This is informational only)
- Added ``get_or_none(...)`` as syntactic sugar for ``filter(...).first()``

0.15.6
------
- Added ``BinaryField`` for storing binary objects (``bytes``).
- Changed ``TextField`` to use ``LONGTEXT`` for MySQL to allow for larger than 64KB of text.
- De-duplicate index if specified on both ``index=true`` and as part of ``indexes``
- Primary Keyed ``TextField`` is marked as deprecated.
  We can't guarantee that it will be properly indexed or unique in all cases.
- One can now disable the backwards relation for FK/O2O relations by passing ``related_name=False``
- One can now pass a PK value to a generated field, and Tortoise ORM will use that as the PK as expected.
  This allows one to have a model that has a autonumber PK, but setting it explicitly if required.

0.15.5
------
* Refactored Fields:

  Fields have been refactored, for better maintenance. There should be no change for most users.

  - More accurate auto-completion.
  - Fields now contain their own SQL schema by dialect, which significantly simplifies adding field types.
  - ``describe_model()`` now returns the DB type, and dialect overrides.

- ``JSONField`` will now automatically use ``python-rapidjson`` as an accelerator if it is available.
- ``DecimalField`` and aggregations on it, now behaves much more like expected on SQLite (#256)
- Check whether charset name is valid for the MySQL connection (#261)
- Default DB driver parameters are now applied consistently, if you use the URI schema or manual.

0.15.4
------
- Don't generate a schema if there is no models (#254)
- Emit a ``RuntimeWarning`` when a module has no models to import (#254)
- Allow passing in a custom SSL context (#255)

0.15.3
------
* Added ``OneToOneField`` implementation:

  ``OneToOneField`` describes a one to one relation between two models.
  It can be set from the primary side only, but resolved from both sides in the same way.

  ``describe_model(...)`` now also reports OneToOne relations in both directions.

  Usage example:

  .. code-block:: python3

     event: fields.OneToOneRelation[Event] = fields.OneToOneField(
         "models.Event", on_delete=fields.CASCADE, related_name="address"
     )

- Prefetching is done concurrently now, sending all prefetch requests at the same time instead of in sequence.
- Enable foreign key enforcement on SQLite for builds where it was optional.

0.15.2
------
- The ``auto_now_add`` argument of ``DatetimeField`` is handled correctly in the SQLite backend.
- ``unique_together`` now creates named constrains, to prevent the DB from auto-assigning a potentially non-unique constraint name.
- Filtering by an ``auto_now`` field doesn't replace the filter value with ``now()`` anymore.

0.15.1
------
- Handle OR'ing a blank ``Q()`` correctly (#240)

0.15.0
-------
New features:
^^^^^^^^^^^^^
- Pooling has been implemented, allowing for multiple concurrent databases and all the benefits that comes with it.
    - Enabled by default for databases that support it (mysql and postgres) with a minimum pool size of 1, and a maximum of 5
    - Not supported by sqlite
    - Can be changed by passing the ``minsize`` and ``maxsize`` connection parameters
- Many small performance tweaks:
    - Overhead of query generation has been reduced by about 6%
    - Bulk inserts are ensured to be wrapped in a transaction for >50% speedup
    - PostgreSQL prepared queries now use a LRU cache for significant >2x speedup on inserts/updates/deletes
- ``DateField`` & ``DatetimeField`` deserializes faster on PostgreSQL & MySQL.
- Optimized ``.values()`` to do less copying, resulting in a slight speedup.
- One can now pass kwargs and ``Q()`` objects as parameters to ``Q()`` objects simultaneously.

Bugfixes:
^^^^^^^^^
- ``indexes`` will correctly map the foreign key if referenced by name.
- Setting DB generated PK in constructor/create generates exception instead of silently being ignored.

Deprecations:
^^^^^^^^^^^^^
- ``start_transaction`` is deprecated, please use ``@atomic()`` or ``async with in_transaction():`` instead.
- **This release brings with it, deprecation of Python 3.6 / PyPy-3.6:**

  This is due to small differences with how the backported ``aiocontextvars`` behaves
  in comparison to the built-in in Python 3.7+.

  There is a known context confusion, specifically regarding nested transactions.


.. rst-class:: emphasize-children

0.14
====

0.14.2
------
- A Field name of ``alias`` is now no longer reserved.
- Restored support for inheriting from Abstract classes. Order is now also deterministic,
  with the inherited classes' fields being placed before the current.

0.14.1
-------
- ``ManyToManyField`` is now a function that has the type of the relation for autocomplete,
  this allows for better type hinting at less effort.
- Added section on adding better autocomplete for relations in editors.

0.14.0
------
.. caution::
   **This release drops support of Python 3.5:**

   Tortoise ORM now requires a minimum of CPython 3.6 or PyPy3.6-7.1

Enhancements:
^^^^^^^^^^^^^
- Models, Fields & QuerySets have significant type annotation improvements,
  leading to better IDE integration and more comprehensive static analysis.
- Fetching records from the DB is now up to 25% faster.
- Database functions ``Trim()``, ``Length()``, ``Coalesce()``, ``Lower()``, ``Upper()`` added to tortoise.functions module.
- Annotations can be selected inside ``Queryset.values()`` and ``Queryset.values_list()`` expressions.
- Added support for Python 3.8
- The Foreign Key property is now ``await``-able as long as one didn't populate it via ``.prefetch_related()``
- One can now specify compound indexes in the ``Meta:`` class using ``indexes``. It works just like ``unique_together``.

Bugfixes:
^^^^^^^^^
- The generated index name now has significantly lower chance of collision.
- The compiled SQL query contains HAVING and GROUP BY only for aggregation functions.
- Fields for FK relations are quoted properly.
- Fields are quoted properly in ``UNIQUE`` statements.
- Fields are quoted properly in ``KEY`` statements.
- Comment Fields are quoted properly in PostgreSQL dialect.
- ``unique_together`` will correctly map the foreign key if referenced by name.

Deprecations:
^^^^^^^^^^^^^
- ``import from tortoise.aggregation`` is deprecated, please do ``import from tortoise.functions`` instead.

Breaking Changes:
^^^^^^^^^^^^^^^^^
- The hash used to make generated indexes unique has changed.
  The old algorithm had a very high chance of collisions,
  the new hash algorithm is much better in this regard.
- Dropped support for Python 3.5

.. rst-class:: emphasize-children

0.13
====

0.13.12
-------
- Reverted "The ``Field`` class now calls ``super().__init__``, so mixins are properly initialised."
  as it was causing issues on Python 3.6.

0.13.11
-------
- Fixed the ``_FieldMeta`` class not to checking if the 1st base class was Field, so would break with mixins.
- The ``Field`` class now calls ``super().__init__``, so mixins are properly initialised.

0.13.10
-------
- Names ForeignKey constraints in a consistent way

0.13.9
------
- Fields can have 2nd base class which makes IDEs know python type (str, int, datetime...) of the field.
- The ``type`` parameter of ``Field.__init__`` is removed, instead we use the 2nd base class
- Foreign keys and indexes are now defined correctly in MySQL so that they take effect as expected
- MySQL now doesn't warn of unsafe index creation anymore

0.13.8
------
- Fixed bug in schema creation for MySQL where non-int PK did not get declared properly (#195)

0.13.7
------
- ``iexact`` filter modifier was implemented. Queries like ``«queryset».filter(name__iexact=...)`` will perform case-insensitive search.

0.13.6
------
- Fix minor bug in ``Model.__init__`` where we raise the wrong error on setting RFK/M2M values directly.
- Fields in ``Queryset.values_list()`` is now in the defined Model order.
- Fields in ``Queryset.values()`` is now in the defined Model order.

0.13.5
------
- Sample Starlette integration
- Relational fields are now lazily constructed via properties instead of in the constructor,
  this results in a significant overhead reduction for Model instantiation with many relationships.

0.13.4
------
- Assigning to the FK field will correctly set the associated db-field
- Reading a nullalble FK field can now be None
- Nullalble FK fields reverse-FK is now also nullable
- Deleting a nullable FK field sets it to None

0.13.3
------
- Fixed installing Tortoise-ORM in non-unicode systems. (#180)
- ``«queryset».update(…)`` now correctly uses the DB-specific ``to_db_value()``
- ``fetch_related(…)`` now correctly encodes non-integer keys.
- ``ForeignKey`` fields of type ``UUIDField`` are now escaped consistently.
- Pre-generated ForeignKey fields (e.g. UUIDField) is now checked for persistence correctly.
- Duplicate M2M ``.add(…)`` now checks using consistent field encoding.
- ``source_field`` Fields are now handled correctly for ordering.
- ``source_field`` Fields are now handled correctly for updating.

0.13.2
------
* Security fixes for ``«model».save()`` & ``«model».delete()``:

  This is now fully parametrized, and these operations are no longer susceptible to escaping issues.

* Performance improvements:

  - Simple update is now ~3-6× faster
  - Partial update is now ~3× faster
  - Delete is now ~2.7x faster

- Fix generated Schema Primary Key for ``BigIntField`` for MySQL and PostgreSQL.
- Added support for using a ``SmallIntField`` as a auto-gen Primary Key.
- Ensure that default PK is added to the top of the attrs.

0.13.1
------
* Model schema now has a discovery API:

  One can call ``Tortoise.describe_models()`` or ``Tortoise.describe_model(<Model>)`` to get
  a full description of the model(s).

  Please see :meth:`tortoise.Tortoise.describe_model` and :meth:`tortoise.Tortoise.describe_models` for more info.

- Fix in generating comments for Foreign Keys in ``MySQL``
- Added schema support for PostgreSQL. Either set  ``"schema": "custom"`` var in ``credentials`` or as a query parameter ``?schema=custom``
- Default MySQL charset to ``utf8mb4``. If a charset is provided it will also force the TABLE charset to the same.

0.13.0
------
.. warning::
   **This release brings with it, deprecation of Python 3.5:**

   We will increase the minimum supported version of Python to 3.6,
   as 3.5 is reaching end-of-life,
   and is missing many useful features for async applications.

   We will discontinue Python 3.5 support on the next major release (Likely 0.14.0)

New Features:
^^^^^^^^^^^^^
- Example Sanic integration along with register_tortoise hook in contrib (#163)
- ``.values()`` and ``.values_list()`` now default to all fields if none are specified.
- ``generate_schema()`` now generates well-formatted DDL SQL statements.
- Added ``TruncationTestCase`` testing class that truncates tables to allow faster testing of transactions.
- Partial saves are now supported (#157): ``obj.save(update_fields=['model','field','names'])``

Bugfixes:
^^^^^^^^^
- Fixed state leak between database drivers which could cause incorrect DDL generation.
- Fixed missing table/column comment generation for ``ForeignKeyField`` and ``ManyToManyField``
- Fixed comment generation to escape properly for ``SQLite``
- Fixed comment generation for ``PostgreSQL`` to not duplicate comments
- Fixed generation of schema for fields that defined custom ``source_field`` values defined
- Fixed working with Models that have fields with custom ``source_field`` values defined
- Fixed safe creation of M2M tables for MySQL dialect (#168)

Docs/examples:
^^^^^^^^^^^^^^
- Examples have been reworked:

  - Simplified init of many examples
  - Re-did ``generate_schema.py`` example
  - A new ``relations_recirsive.py`` example (turned into test case)

- Lots of small documentation cleanups


.. rst-class:: emphasize-children

0.12
====

0.12.7 (retracted)
------------------
- Support connecting to PostgreSQL via Unix domain socket (simple case).
- Self-referential Foreign and Many-to-Many keys are now allowed

0.12.6 / 0.12.8
---------------
* Handle a ``__models__`` variable within modules to override the model discovery mechanism.

    If you define the ``__models__`` variable in ``yourapp.models`` (or wherever you specify to load your models from),
    ``generate_schema()`` will use that list, rather than automatically finding all models for you.

- Split model constructor into from-Python and from-DB paths, leading to 15-25% speedup for large fetch operations.
- More efficient queryset manipulation, 5-30% speedup for small fetches.

0.12.5
------
- Using non registered models or wrong references causes an ConfigurationError with a helpful message.

0.12.4
------
- Inherit fields from Mixins, together with abstract model classes.

0.12.3
------
- Added description attribute to Field class. (#124)
- Added the ability to leverage field description from (#124) to generate table column comments and ability to add table level comments

0.12.2
------
- Fix accidental double order-by for ``.values()`` based queries. (#143)

0.12.1
------
* Bulk insert operation:

  .. note::
     The bulk insert operation will do the minimum to ensure that the object
     created in the DB has all the defaults and generated fields set,
     this may result in incomplete references in Python.

     e.g. ``IntField`` primary keys will not be populated.

  This is recommend only for throw away inserts where you want to ensure optimal
  insert performance.

  .. code-block:: python3

      User.bulk_create([
          User(name="...", email="..."),
          User(name="...", email="...")
      ])

- Notable efficiency improvement for regular inserts

0.12.0
------
* Tortoise ORM now supports non-autonumber primary keys.

  .. note::
     This is a big feature change. It should not break any existing implementations.

  That primary key will be accessible through a reserved field ``pk`` which will be an alias of whichever field has been nominated as a primary key.
  That alias field can be used as a field name when doing filtering e.g. ``.filter(pk=...)`` etc…

  We currently support single (non-composite) primary keys of any indexable field type, but only these field types are recommended:

  .. code-block:: python3

      IntField
      BigIntField
      CharField
      UUIDField

  One must define a primary key by setting a ``pk`` parameter to ``True``.

  If you don't define a primary key, we will create a primary key of type ``IntField`` with name of ``id`` for you.

  Any of these are valid primary key definitions in a Model:

  .. code-block:: python3

      id = fields.IntField(pk=True)

      checksum = fields.CharField(pk=True)

      guid = fields.UUIDField(pk=True)


.. rst-class:: emphasize-children

0.11
====

0.11.13
-------
- Fixed connection retry to work with transactions
- Added broader PostgreSQL connection failure detection

0.11.12
-------
- Added automatic PostgreSQL connection retry

0.11.11
-------
- Extra parameters now get passed through to the MySQL & PostgreSQL drivers

0.11.10
-------
- Fixed SQLite handling of DatetimeField

0.11.9
------
- Code has been reformatted using ``black``, and minor code cleanups (#120 #123)
- Sample Quart integration (#121)
- Better isolation of connection handling — Allows more dynamic connections so we can do pooling & reconnections.
- Added automatic MySQL connection retry

0.11.8
------
- Fixed ``.count()`` when a join happens (#109)

0.11.7
------
- Fixed ``unique_together`` for foreign keys (#114)
- Fixed Field.to_db_value method to handle Enum (#113 #115 #116)

0.11.6
------
- Added ability to use ``unique_together`` meta Model option

0.11.5
------
- Fixed concurrency isolation when attempting to do multiple concurrent operations on a single connection.

0.11.4
------
- Fixed several convenience issues with foreign relations:

  - FIXED: ``.all()`` actually returns the _query property as was documented.
  - New models with FK don't automatically fail to resolve any data. They can now be evaluated lazily.

- Some DB's don't support OFFSET without Limit, added caps to signal workaround, which is to automatically add limit of 1000000
- Pylint plugin to know about default ``related_name`` for ForeignKey fields.
- Simplified capabilities to be static, and defined at class level.

0.11.3
------
* Added basic DB driver Capabilities.

  Test runner now has the ability to skip tests conditionally, based on the DB driver Capabilities:

  .. code-block:: python3

      @requireCapability(dialect='sqlite')
      async def test_run_sqlite_only(self):
          ...

* Added per-field indexes.

  When setting ``index=True`` on a field, Tortoise will now generate an index for it.

  .. note::
     Due to MySQL limitation of not supporting conditional index creation,
     if ``safe=True`` (the default) is set, it won't create the index and emit a warning about it.

     We plan to work around this limitation in a future release.

- Performance fix with PyPika for small fetch queries
- Remove parameter hack now that PyPika support Parametrized queries
- Fix typos in JSONField docstring
- Added ``.explain()`` method on ``QuerySet``.
- Add ``required`` read-only property to fields

0.11.2
------
- Added "safe" schema generation
- Correctly convert values to their db representation when using the "in" filter
- Added some common missing field types:

  - ``BigIntField``
  - ``TimeDeltaField``

- ``BigIntField`` can also be used as a primary key field.

0.11.1
------
- Test class isolation fixes & contextvars update
- Turned on autocommit for MySQL
- db_url now supports defaults and casting parameters to the right types

0.11.0
------
- Added ``.exclude()`` method for QuerySet
- Q objects can now be negated for ``NOT`` query (``~Q(...)``)
- Support subclassing on existing fields
- Numerous bug fixes
- Removed known broken connection pooling

.. rst-class:: emphasize-children

0.10
====

0.10.11
-------
- Pre-build some query & filters statically, 15-30% speed up for smaller queries.
- Required field params are now positional, so Python and IDE linters will pick up on it easier.
- Filtering also applies DB-specific transforms, Fixes #62
- Fixed recursion error on m2m management with big lists

0.10.10
-------
- Refactor ``Tortoise.init()`` and test runner to not re-create connections per test, so now tests pass when using an SQLite in-memory database
- Can pass event loop to test initializer function: ``initializer(loop=loop)``
- Fix relative URI for SQLite
- Better error message for invalid filter param.
- Better error messages for missing/bad field params.
- ``nose2`` plugin
- Test utilities compatible with ``py.test``

0.10.9
------
- Uses macros on SQLite driver to minimise syncronisation. ``aiosqlite>=0.7.0``
- Uses prepared statements for insert, large insert performance increase.
- Pre-generate base pypika query object per model, providing general purpose speedup.

0.10.8
------
- Performance fixes from ``pypika>=0.15.6``
- Significant reduction in object creation time

0.10.7
------
- Fixed SQLite relative db path and :memory: now also works
- Removed confusing error message for missing db driver dependency
- Added ``aiosqlite`` as a required dependency
- ``execute_script()`` now annotates errors just like ``execute_query()``, to reduce confusion
- Bumped ``aiosqlite>=0.6.0`` for performance fix
- Added ``tortoise.run_async()`` helper function to make smaller scripts easier to run. It cleans up connections automatically.
- SQLite does autocommit by default.

0.10.6
------
- Fixed atomic decorator to get connection only on function call

0.10.5
------
- Fixed pre-init queryset objects creation

0.10.4
------
- Added support for running separate transactions in multidb config

0.10.3
------
- Changed default app label from 'models' to None
- Fixed ConfigurationError message for wrong connection name

0.10.2
------
- Set single_connection to True by default, as there is known issues with connection pooling
- Updated documentation

0.10.1
------
- Fixed M2M manager methods to correctly work with transactions
- Fixed mutating of queryset on select queries

0.10.0
------
* Refactored ``Tortoise.init()`` to init all connections and discover models from config passed
  as argument.

  .. caution::
     This is a breaking change.

  You no longer need to import the models module for discovery,
  instead you need to provide an app ⇒ modules map with the init call:

  .. code-block:: python3

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

  For more info, please have a look at :ref:`init_app`

- New ``transactions`` module for implicit working with transactions
- Test frameworks overhauled:
  - Better performance for test runner, using transactions to keep tests isolated.
  - Now depends on an ``initializer()`` and ``finalizer()`` to set up and tear down DB state.
- Exceptions have been further clarified
- Support for CPython 3.7
- Added support for MySQL/MariaDB


.. rst-class:: emphasize-children

0.9 & older
===========

0.9.4
-----
- No more asserts, only Tortoise Exceptions
- Fixed PyLint plugin to work with pylint>=2.0.0
- Formalised unittest classes & documented them.
- ``__slots__`` where it was easy to do. (Changes class instances from dicts into tuples, memory savings)

0.9.3
-----
- Fixed backward incompatibility for Python 3.7

0.9.2
-----
- ``JSONField`` is now promoted to a standard field.
- Fixed ``DecimalField`` and ``BooleanField`` to work as expected on SQLite.
- Added ``FloatField``.
- Minimum supported version of PostgreSQL is 9.4
- Added ``.get(...)`` shortcut on query set.
- ``values()`` and ``values_list()`` now converts field values to python types

0.9.1
-----
- Fixed ``through`` parameter honouring for ``ManyToManyField``

0.9.0
-----
* Added support for nested queries for ``values`` and ``values_list``:

  .. code-block:: python3

      result = await Event.filter(id=event.id).values('id', 'name', tournament='tournament__name')
      result = await Event.filter(id=event.id).values_list('id', 'participants__name')

- Fixed ``DatetimeField`` and ``DateField`` to work as expected on SQLite.
- Added ``PyLint`` plugin.
- Added test class to mange DB state for testing isolation.

0.8.0
-----
- Added PostgreSQL ``JSONField``

0.7.0
-----
- Added ``.annotate()`` method and basic aggregation funcs

0.6.0
-----
- Added ``Prefetch`` object

0.5.0
-----
- Added ``contains`` and other filter modifiers.
- Field kwarg ``default`` now accepts functions.

0.4.0
-----
- Immutable QuerySet. ``unique`` flag for fields

0.3.0
-----
* Added schema generation and more options for fields:

  .. code-block:: python3

      from tortoise import Tortoise
      from tortoise.backends.sqlite.client import SqliteClient
      from tortoise.utils import generate_schema

      client = SqliteClient(db_name)
      await client.create_connection()
      Tortoise.init(client)
      await generate_schema(client)

0.2.0
-----
* Added filtering and ordering by related models fields:

  .. code-block:: python3

      await Tournament.filter(
          events__name__in=['1', '3']
      ).order_by('-events__participants__name').distinct()
