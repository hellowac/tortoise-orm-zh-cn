==================
贡献指南
==================

**Contribution Guide**

.. toctree::
   :hidden:

   CODE_OF_CONDUCT

.. md-tab-set::
    
    .. md-tab-item:: 中文

        如果您想贡献，请查看问题，或者直接创建 PR。

        Tortoise ORM 是一项志愿者工作。我们鼓励您参与并加入团队！

        请查看 :ref:`code_conduct`
    
    .. md-tab-item:: 英文


        If you want to contribute check out issues, or just straightforwardly create PR.

        Tortoise ORM is a volunteer effort. We encourage you to pitch in and join the team!

        Please have a look at the :ref:`code_conduct`


提交错误或请求功能
====================================

**Filing a bug or requesting a feature**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        请检查是否已有相关问题或拉取请求在处理您的问题。如果没有，欢迎您提交一个新的请求。

        如果您有一个未完成的更改，但无法继续进行，请务必创建一个拉取请求并标记为 ``(WIP)``，以便我们可以相互帮助。
    
    .. md-tab-item:: 英文

        Please check if there isn't an existing issue or pull request open that addresses your issue.
        If not you are welcome to open one.

        If you have an incomplete change, but won't/can't continue working on it, please create a PR in any case and mark it as ``(WIP)`` so we can help each other.


聊天
===========

**Have a chat**

.. md-tab-set::
    
    .. md-tab-item:: 中文
        
        我们在 `Gitter <https://gitter.im/tortoise/community>`_ 上有一个聊天室

    .. md-tab-item:: 英文

        We have a chatroom on `Gitter <https://gitter.im/tortoise/community>`_


项目结构
=================

**Project structure**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们有一个 `Makefile`，列出了常见操作，开始时只需输入 `make`::

            Tortoise ORM 开发 Makefile

            用法：make <目标>
            目标：
                up          更新开发/测试依赖
                deps        确保安装开发/测试依赖
                check       检查构建是否正常
                lint        报告所有 linter 违规
                test        运行所有测试
                docs        构建文档
                style       自动格式化代码

        所以要运行测试，您只需运行 `make test` 等等…

        代码结构如下：

        ``docs/``:
            文档
        ``examples/``:
            示例代码
        ``tortoise/``:
            基础 Tortoise ORM
        ``tortoise/fields/``:
            字段在这里定义。
        ``tortoise/backends/``:
            数据库后端，如 `sqlite`、`asyncpg`、`psycopg` 和 `mysql`
        ``tortoise/backends/base/``:
            通用数据库后端代码
        ``tortoise/contrib/``:
            任何帮助人们使用该项目的内容，如测试框架和 linter 插件
        ``tortoise/tests/``:
            Tortoise 测试代码
    
    .. md-tab-item:: 英文

        We have a ``Makefile`` that has the common operations listed, to get started just type ``make``::

            Tortoise ORM development makefile

            usage: make <target>
            Targets:
                up          Updates dev/test dependencies
                deps        Ensure dev/test dependencies are installed
                check       Checks that build is sane
                lint        Reports all linter violations
                test        Runs all tests
                docs        Builds the documentation
                style       Auto-formats the code

        So to run the tests you just need to run ``make test``, etc…

        The code is structured in the following directories:

        ``docs/``:
            Documentation
        ``examples/``:
            Example code
        ``tortoise/``:
            Base Tortoise ORM
        ``tortoise/fields/``:
            The Fields are defined here.
        ``tortoise/backends/``:
            DB Backends, such as ``sqlite``, ``asyncpg``, ``psycopg`` & ``mysql``
        ``tortoise/backends/base/``:
            Common DB Backend code
        ``tortoise/contrib/``:
            Anything that helps people use the project, such as Testing framework and linter plugins
        ``tortoise/tests/``:
            The Tortoise test code


编码指南
================

**Coding Guideline**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们相信保持代码简单，这样就更容易测试，而且隐藏错误的地方也更少。
    
    .. md-tab-item:: 英文

        We believe to keep the code simple, so it is easier to test and there is less place for bugs to hide.

优先事项
----------

**Priorities**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 的一个重要部分是我们希望提供一个简单的接口，仅执行预期的功能。由于这一价值观因人而异，我们确定了以下原则：

        * 模型/查询集的使用应明确且简洁。
        * 保持简单，因为简单的代码通常不仅运行更快，而且错误也更少。
        * 正确性 > 易用性 > 性能 > 可维护性
        * 测试所有内容。
        * 仅在有可重复的基准测试可供测量时进行性能/内存优化。
    
    .. md-tab-item:: 英文

        An important part of Tortoise ORM is that we want a simple interface, that only does what is expected.
        As this is a value that is different for different people, we have settled on:

        * Model/QuerySet usage should be explicit and concise.
        * Keep it simple, as simple code not only often runs faster, but has less bugs too.
        * Correctness > Ease-Of-Use > Performance > Maintenance
        * Test everything.
        * Only do performance/memory optimisation when you have a repeatable benchmark to measure with.


样式
-----

**Style**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        我们尽量自动化所有可能的操作，因此简单地运行 ``make check`` 将进行自动化的样式检查和 linting，但这些工具无法检测出不明显的样式偏好。

        Tortoise ORM 遵循以下约定的风格：

        * 尽可能遵循 PEP8。
        * 最大行长度更改为 100。
        * 始终尝试清晰地分隔术语，而不是直接连接单词：
            * 使用 ``some_purpose`` 而不是 ``somepurpose``。
            * 使用 ``SomePurpose`` 而不是 ``Somepurpose``。
        * 请记住目标 Python 版本为 ``>=3.8``：
            * 使用 f-string。
        * 请尽量提供类型注解，这将改善编辑器中的自动补全和更好的静态分析。
    
    .. md-tab-item:: 英文

        We try and automate as much as we can, so a simple ``make check`` will do automated style checking and linting, but these don't pick up on the non-obvious style preferences.

        Tortoise ORM follows a the following agreed upon style:

        * Keep to PEP8 where you can
        * Max line-length is changed to 100
        * Always try to separate out terms clearly rather than concatenate words directly:
            * ``some_purpose`` instead of ``somepurpose``
            * ``SomePurpose`` instead of ``Somepurpose``
        * Keep in mind the targeted Python versions of ``>=3.8``:
            * Do use f-strings
        * Please try and provide type annotations where you can, it will improve auto-completion in editors, and better static analysis.


运行测试
================

**Running tests**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在 Windows 上原生运行测试尚不支持（暂时）。目前运行测试的最佳方法是使用 WSL（Windows Subsystem for Linux）。Postgres 使用默认的 ``postgres`` 用户，MySQL 使用 ``root``。如果其中任何一个有密码，可以分别通过环境变量 ``TORTOISE_POSTGRES_PASS`` 和 ``TORTOISE_MYSQL_PASS`` 设置密码。
    
    .. md-tab-item:: 英文

        Running tests natively on windows isn't supported (yet). Best way to run them atm is by using WSL.
        Postgres uses the default ``postgres`` user, mysql uses ``root``. If either of them has a password you can set it with the ``TORTOISE_POSTGRES_PASS`` and ``TORTOISE_MYSQL_PASS`` env variables respectively.



测试的不同种类
-----------------------------

**Different types of tests**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - ``make test``：最基本的快速测试，仅在内存 SQLite 数据库中运行测试，不生成覆盖率报告。
        - ``make test_sqlite``：在 SQLite 内存数据库上运行测试。
        - ``make test_postgres_asyncpg``：在 PostgreSQL 数据库上运行 asyncpg 测试。
        - ``make test_postgres_psycopg``：在 PostgreSQL 数据库上运行 psycopg 测试。
        - ``make test_mysql_myisam``：在使用 ``MYISAM`` 存储引擎的 MySQL 数据库上运行测试（无事务）。
        - ``make test_mysql``：在 MySQL 数据库上运行测试。
        - ``make testall``：在所有四种数据库类型上运行测试：SQLite（内存）、PostgreSQL、MySQL-MyISAM 和 MySQL-InnoDB。
        - ``green``：运行与 ``make test`` 相同的测试，确保 green 插件正常工作。
        - ``nose2 --plugin tortoise.contrib.test.nose2 --db-module tests.testmodels --db-url sqlite://:memory:``：与 ``make test`` 相同的测试，确保 nose2 插件正常工作。
    
    .. md-tab-item:: 英文

        - ``make test``: most basic quick test. only runs the tests on in an memory sqlite database without generating a coverage report.
        - ``make test_sqlite``: Runs the tests on a sqlite in memory database
        - ``make test_postgres_asyncpg``: Runs the asyncpg tests on the postgres database
        - ``make test_postgres_psycopg``: Runs the psycopg tests on the postgres database
        - ``make test_mysql_myisam``: Runs the tests on the mysql database using the ``MYISAM`` storage engine (no transactions)
        - ``make test_mysql``: Runs the tests on the mysql database
        - ``make testall``: runs the tests on all 4 database types: sqlite (in memory), postgresql, MySQL-MyISAM and MySQL-InnoDB
        - ``green``: runs the same tests as ``make test``, ensures the green plugin works
        - ``nose2 --plugin tortoise.contrib.test.nose2 --db-module tests.testmodels --db-url sqlite://:memory: ``: same test as ``make test`` , ensures the nose2 plugin works


运行测试套件时需要注意的事项
---------------------------------------------------

**Things to be aware of when running the test suite**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        - 一些测试会始终运行，无论您正在运行哪个测试套件（例如，MySQL 和 PostgreSQL 的连接测试，您不需要数据库运行，因为它实际上不会连接）。
        - 一些测试使用硬编码的数据库（通常是 SQLite）进行测试，无论您指定了什么 DB URL。
        - PostgreSQL 驱动程序在 Pypy 下无法工作，因此在 Pypy 下运行时这些测试将被跳过。
        - 您可以通过运行 ``py.test <testfiles>`` 或 ``green -s 1 <testfile>`` 来仅运行特定的测试。
        - 如果您想查看挂起测试的底层情况以进行调试，可以使用 ``green -s 1 -vv -d -a <test>`` 运行它们：
            - ``-s 1`` 表示每次仅运行一个测试。
            - ``-vv`` 生成非常详细的输出。
            - ``-d`` 记录调试输出。
            - ``-a`` 不捕获 stdout，而是直接输出。
        - MySQL 的速度相对较慢，但您可以调整一些设置来提高速度，但这也意味着减少冗余。使用需谨慎：<http://www.tocker.ca/2013/11/04/reducing-mysql-durability-for-testing.html>
    
    .. md-tab-item:: 英文

        - Some tests always run regardless of what test suite you are running (the connection tests for mysql and postgres for example, you don't need a database running as it doesn't actually connect though)
        - Some tests use hardcoded databases (usually sqlite) for testing, regardless of what DB url you specified.
        - The postgres driver does not work under Pypy so those tests will be skipped if you are running under pypy
        - You can run only specific tests by running `` py.test <testfiles>`` or ``green -s 1 <testfile>``
        - If you want a peek under the hood of test that hang to debug try running them with ``green -s 1 -vv -d -a <test>``
            - ``-s 1`` means it only runs one test at a time
            - ``-vv`` very verbose output
            - ``-d`` log debug output
            - ``-a`` don't capture stdout but just let it output
        - Mysql tends to be relatively slow but there are some settings you can tweak to make it faster, however this also means less redundant. Use at own risk: http://www.tocker.ca/2013/11/04/reducing-mysql-durability-for-testing.html
