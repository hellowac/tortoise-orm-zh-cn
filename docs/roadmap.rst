=======
路线图
=======

**Roadmap**

短期计划
==========

**Short-term**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们的短期目标是将当前实现作为 MVP 交付，只是稍微成熟一些。

        对于 ``v1.0`` ，涉及：

        * 时区支持
    
    .. md-tab-item:: 英文

        Our short term goal is to ship the current implementation as MVP, just somewhat matured.

        For ``v1.0`` that involves:

        * Timezone support

中期计划
========

**Mid-term**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        这里列出了所有稍远期的功能，顺序不分先后：

        * 性能优化：
            * 子查询
            * 全参数化查询的更改
            * 更快的 MySQL 驱动（可能基于 mysqlclient）
            * 考虑使用 Cython 来加速关键循环

        * 便利性/易用性改进：
            * 让 ``DELETE`` 支持 ``limit`` 和 ``offset``
            * 使 ``.filter(field=None)`` 的行为符合预期

        * 扩展 ``init`` 框架：
            * 提供管理命令的能力
            * 能够定义管理命令
            * 简化模型和管理命令的检查，无需使用私有 API

        * 迁移：
            * 完备的模式迁移
            * 自动前向迁移构建
            * 能够在迁移中轻松运行任意代码
            * 能够获取迁移时的确切模型，以确保安全和一致的数据迁移
            * 跨数据库支持
            * 将 Fixtures 作为迁移的一个属性

        * 序列化支持：
            * 添加反序列化支持
            * 使默认序列化器支持某种验证
            * 提供替换序列化器为自定义解决方案的简洁方法

        * 额外的数据库支持：
            * CockroachDB
            * Firebird

        * 增强的测试支持：
            * ``hypothesis`` 策略构建器

        * 字段：
            * 扩展提供的标准字段

        * 文档：
            * 教程

    .. md-tab-item:: 英文

        Here we have all the features that is slightly further out, in no particular order:

        * Performance work:
            * Sub queries
            * Change to all-parametrized queries
            * Faster MySQL driver (possibly based on mysqlclient)
            * Consider using Cython to accelerate critical loops

        * Convenience/Ease-Of-Use work:
            * Make ``DELETE`` honour ``limit`` and ``offset``
            * ``.filter(field=None)`` to work as expected

        * Expand in the ``init`` framework:
            * Ability to have Management Commands
            * Ability to define Management Commands
            * Make it simple to inspect Models and Management Commands without using private APIs.

        * Migrations
            * Comprehensive schema Migrations
            * Automatic forward Migration building
            * Ability to easily run arbitrary code in a migration
            * Ability to get a the Models for that exact time of the migration, to ensure safe & consistent data migrations
            * Cross-DB support
            * Fixtures as a property of a migration

        * Serialization support
            * Add deserialization support
            * Make default serializers support some validation
            * Provide clean way to replace serializers with custom solution

        * Extra DB support
            * CockroachDB
            * Firebird

        * Enhanced test support
            * ``hypothesis`` strategy builder

        * Fields
            * Expand on standard provided fields

        * Documentation
            * Tutorials

长期计划
=========

**Long-term**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        成为事实上的 Python AsyncIO ORM。
    
    .. md-tab-item:: 英文

        Become the de facto Python AsyncIO ORM.
