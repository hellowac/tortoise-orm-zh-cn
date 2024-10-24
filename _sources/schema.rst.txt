:tocdepth: 3

.. _schema:

===============
Schema 创建
===============

**Schema Creation**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        这里我们创建一个与 SQLite 数据库客户端的连接，然后发现并初始化模型。

        .. automethod:: tortoise.Tortoise.generate_schemas
            :noindex:

        ``generate_schema`` 在空数据库上生成架构。
        生成架构时还有一个默认选项，即将 ``safe`` 参数设置为 ``True``，这将仅在表不存在时插入表。

    .. md-tab-item:: 英文

        Here we create connection to SQLite database client and then we discover & initialize models.

        .. automethod:: tortoise.Tortoise.generate_schemas
            :noindex:

        ``generate_schema`` generates schema on empty database.
        There is also the default option when generating the schemas to set the ``safe`` parameter to ``True`` which will only insert the tables if they don't already exist.


帮助函数
================

**Helper Functions**

.. automodule:: tortoise.utils
    :members: get_schema_sql, generate_schema_for_client
