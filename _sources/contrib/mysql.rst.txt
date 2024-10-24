:tocdepth: 4

.. _contrib_mysql:

=====
MySQL
=====

索引
=======

**Indexes**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        MySQL 特定索引。
    
    .. md-tab-item:: 英文

        MySQL specific indexes.

.. autoclass:: tortoise.contrib.mysql.indexes.FullTextIndex
.. autoclass:: tortoise.contrib.mysql.indexes.SpatialIndex

字段
======

**Fields**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        MySQL 特定字段。
    
    .. md-tab-item:: 英文

        MySQL specific fields.

.. autoclass:: tortoise.contrib.mysql.fields.GeometryField
.. autoclass:: tortoise.contrib.mysql.fields.UUIDField

搜索
======

**Search**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        MySQL 全文搜索。
    
    .. md-tab-item:: 英文

        MySQL full text search.

.. autoclass:: tortoise.contrib.mysql.search.SearchCriterion
