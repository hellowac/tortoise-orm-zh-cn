:tocdepth: 4

.. _contrib_postgre:

========
Postgres
========

索引
=======

**Indexes**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Postgres 特定索引。
    
    .. md-tab-item:: 英文

        Postgres specific indexes.

.. autoclass:: tortoise.contrib.postgres.indexes.BloomIndex
.. autoclass:: tortoise.contrib.postgres.indexes.BrinIndex
.. autoclass:: tortoise.contrib.postgres.indexes.GinIndex
.. autoclass:: tortoise.contrib.postgres.indexes.GistIndex
.. autoclass:: tortoise.contrib.postgres.indexes.HashIndex
.. autoclass:: tortoise.contrib.postgres.indexes.SpGistIndex

字段
======

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Postgres 特定字段。
    
    .. md-tab-item:: 英文

        Postgres specific fields.

.. autoclass:: tortoise.contrib.postgres.fields.ArrayField
.. autoclass:: tortoise.contrib.postgres.fields.TSVectorField


函数
=========

**Functions**

.. autoclass:: tortoise.contrib.postgres.functions.ToTsVector
.. autoclass:: tortoise.contrib.postgres.functions.ToTsQuery
.. autoclass:: tortoise.contrib.postgres.functions.PlainToTsQuery

搜索
======

**Search**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Postgres 全文搜索。
    
    .. md-tab-item:: 英文

        Postgres full text search.

.. autoclass:: tortoise.contrib.postgres.search.SearchCriterion
