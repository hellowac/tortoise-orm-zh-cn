:tocdepth: 3

========
时区
========

**Timezone**

.. _timezone:

前言
============

**Introduction**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        时区的设计灵感来源于 `Django`，但也存在差异。Tortoise 中有两个配置项 `use_tz` 和 `timezone`，它们影响时区设置，可以在调用 `Tortoise.init` 时进行设置。而且，不同的数据库管理系统（DBMS）也有不同的行为。

    .. md-tab-item:: 英文

        The design of timezone is inspired by `Django` but also has differences. There are two config items `use_tz` and `timezone` affect timezone in tortoise, which can be set when call `Tortoise.init`. And in different DBMS there also are different behaviors.

use_tz
------

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        当将 `use_tz` 设置为 `True` 时，`tortoise` 会始终在数据库中存储 `UTC` 时间，无论设置了什么 `timezone`。在生成模式时，`MySQL` 使用字段类型 `DATETIME(6)`，`PostgreSQL` 使用 `TIMESTAMPTZ`，`SQLite` 使用 `TIMESTAMP`。对于 `TimeField`，`MySQL` 使用 `TIME(6)`，`PostgreSQL` 使用 `TIMETZ`，而 `SQLite` 使用 `TIME`。

    .. md-tab-item:: 英文

        When set `use_tz = True`, `tortoise` will always store `UTC` time in database no matter what `timezone` set. And `MySQL` use field type `DATETIME(6)`, `PostgreSQL` use `TIMESTAMPTZ`, `SQLite` use `TIMESTAMP` when generate schema.
        For `TimeField`, `MySQL` use `TIME(6)`, `PostgreSQL` use `TIMETZ` and `SQLite` use `TIME`.

timezone
--------

.. md-tab-set::
    
    .. md-tab-item:: 中文
        
        `timezone` 确定从数据库中选择 `DateTimeField` 和 `TimeField` 时使用的时区，无论您的数据库是什么时区。您应该使用 `tortoise.timezone.now()` 来获取具有时区感知的时间，而不是使用本地时间 `datetime.datetime.now()`。

    .. md-tab-item:: 英文

        The `timezone` determine what `timezone` is when select `DateTimeField` and `TimeField` from database, no matter what `timezone` your database is. And you should use `tortoise.timezone.now()` get aware time instead of native time `datetime.datetime.now()`.

参考
=========

**Reference**

.. automodule:: tortoise.timezone
    :members:
    :undoc-members:
