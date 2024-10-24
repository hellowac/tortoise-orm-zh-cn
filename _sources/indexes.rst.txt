:tocdepth: 4

.. _index:

=======
索引
=======

**Indexes**

.. md-tab-set::
    
    .. md-tab-item:: 中文
        
        默认情况下，Tortoise 在定义字段时使用 `db_index=True` 创建 `BTree` 索引，或者在 `Meta` 类中定义索引。如果您想使用其他索引类型，例如 MySQL 中的 `FullTextIndex` 或 Postgres 中的 `GinIndex` ，则应使用 :py:class:`tortoise.indexes.Index` 及其子类。

    .. md-tab-item:: 英文

        Default tortoise use `BTree` index when define index use `db_index=True` in field, or define indexes use in `Meta` class, but if you want use other index types, like `FullTextIndex` in `MySQL`, or `GinIndex` in `Postgres`, you should use `tortoise.indexes.Index` and its subclasses.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        以下是使用 MySQL 的 `FullTextIndex` 和 `SpatialIndex` 的示例：

        .. code-block:: python3

            from tortoise import Model, fields
            from tortoise.contrib.mysql.fields import GeometryField
            from tortoise.contrib.mysql.indexes import FullTextIndex, SpatialIndex


            class Index(Model):
                full_text = fields.TextField()
                geometry = GeometryField()

                class Meta:
                    indexes = [
                        FullTextIndex(fields={"full_text"}, parser_name="ngram"),
                        SpatialIndex(fields={"geometry"}),
                    ]

        一些内置索引可以在 `tortoise.contrib.mysql.indexes` 和 `tortoise.contrib.postgres.indexes` 中找到。
    
    .. md-tab-item:: 英文

        Following is example which use `FullTextIndex` and `SpatialIndex` of `MySQL`:

        .. code-block:: python3

            from tortoise import Model, fields
            from tortoise.contrib.mysql.fields import GeometryField
            from tortoise.contrib.mysql.indexes import FullTextIndex, SpatialIndex


            class Index(Model):
                full_text = fields.TextField()
                geometry = GeometryField()

                class Meta:
                    indexes = [
                        FullTextIndex(fields={"full_text"}, parser_name="ngram"),
                        SpatialIndex(fields={"geometry"}),
                    ]

        Some built-in indexes can be found in `tortoise.contrib.mysql.indexes` and `tortoise.contrib.postgres.indexes`.

扩展索引
===============

**Extending Index**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        扩展索引很简单，您只需继承 `tortoise.indexes.Index`，以下是创建 `FullTextIndex` 的示例：

        .. code-block:: python3

            from typing import Optional, Set
            from pypika.terms import Term
            from tortoise.indexes import Index

            class FullTextIndex(Index):
                INDEX_TYPE = "FULLTEXT"

                def __init__(
                    self,
                    *expressions: Term,
                    fields: Optional[Set[str]] = None,
                    name: Optional[str] = None,
                    parser_name: Optional[str] = None,
                ):
                    super().__init__(*expressions, fields=fields, name=name)
                    if parser_name:
                        self.extra = f" WITH PARSER {parser_name}"

        对于 `Postgres`，您应继承 `tortoise.contrib.postgres.indexes.PostgresIndex`：

        .. code-block:: python3

            class BloomIndex(PostgreSQLIndex):
                INDEX_TYPE = "BLOOM"

    .. md-tab-item:: 英文

        Extending index is simply, you just need to inherit the `tortoise.indexes.Index`, following is example how to create `FullTextIndex`:

        .. code-block:: python3

            from typing import Optional, Set
            from pypika.terms import Term
            from tortoise.indexes import Index

            class FullTextIndex(Index):
                INDEX_TYPE = "FULLTEXT"

                def __init__(
                    self,
                    *expressions: Term,
                    fields: Optional[Set[str]] = None,
                    name: Optional[str] = None,
                    parser_name: Optional[str] = None,
                ):
                    super().__init__(*expressions, fields=fields, name=name)
                    if parser_name:
                        self.extra = f" WITH PARSER {parser_name}"

        Differently for `Postgres`, you should inherit `tortoise.contrib.postgres.indexes.PostgresIndex`:

        .. code-block:: python3

            class BloomIndex(PostgreSQLIndex):
                INDEX_TYPE = "BLOOM"
