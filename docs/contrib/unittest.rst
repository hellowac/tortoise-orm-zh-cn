:tocdepth: 3

.. _unittest:

================
单元测试支持
================

**UnitTest support**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise ORM 包含自己的辅助实用程序来协助单元测试。
    
    .. md-tab-item:: 英文

        Tortoise ORM includes its own helper utilities to assist in unit tests.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: python3

            from tortoise.contrib import test

            class TestSomething(test.TestCase):
                def test_something(self):
                    ...

                async def test_something_async(self):
                    ...

                @test.skip('跳过此测试')
                def test_skip(self):
                    ...

                @test.expectedFailure
                def test_something(self):
                    ...


        要使 ``test.TestCase`` 按预期工作，您需要配置测试环境的设置和清理，以调用以下内容：

        .. code-block:: python3

            from tortoise.contrib.test import initializer, finalizer

            # 在设置中
            initializer(['module.a', 'module.b.c'])
            # 可选的 db_url、app_label 和 loop 参数
            initializer(['module.a', 'module.b.c'], db_url='...', app_label="someapp", loop=loop)
            # 或者通过环境变量驱动 → 请参阅下面的 Green 测试运行器部分。
            env_initializer()

            # 在清理中
            finalizer()


        关于 DB_URL，应遵循以下标准：

            TORTOISE_TEST_DB=sqlite:///tmp/test-{}.sqlite
            TORTOISE_TEST_DB=postgres://postgres:@127.0.0.1:5432/test_{}


        ``{}`` 是一个字符串替换参数，将创建一个随机化的数据库名称。
        这目前是 ``test.IsolatedTestCase`` 功能所必需的。
        如果您不使用 ``test.IsolatedTestCase``，那么可以提供绝对地址。
        SQLite 内存数据库 ``:memory:`` 将始终有效，并且是默认值。

    .. md-tab-item:: 英文

        .. code-block:: python3

            from tortoise.contrib import test

            class TestSomething(test.TestCase):
                def test_something(self):
                    ...

                async def test_something_async(self):
                    ...

                @test.skip('Skip this')
                def test_skip(self):
                    ...

                @test.expectedFailure
                def test_something(self):
                    ...


        To get ``test.TestCase`` to work as expected, you need to configure your test environment setup and teardown to call the following:

        .. code-block:: python3

            from tortoise.contrib.test import initializer, finalizer

            # In setup
            initializer(['module.a', 'module.b.c'])
            # With optional db_url, app_label and loop parameters
            initializer(['module.a', 'module.b.c'], db_url='...', app_label="someapp", loop=loop)
            # Or env-var driven → See Green test runner section below.
            env_initializer()

            # In teardown
            finalizer()


        On the DB_URL it should follow the following standard:

            TORTOISE_TEST_DB=sqlite:///tmp/test-{}.sqlite
            TORTOISE_TEST_DB=postgres://postgres:@127.0.0.1:5432/test_{}


        The ``{}`` is a string-replacement parameter, that will create a randomized database name.
        This is currently required for ``test.IsolatedTestCase`` to function.
        If you don't use ``test.IsolatedTestCase`` then you can give an absolute address.
        The SQLite in-memory ``:memory:`` database will always work, and is the default.

.. rst-class:: emphasize-children

测试运行器
============

**Test Runners**

绿色
----------

**Green**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在您的 ``.green`` 文件中：

        .. code-block:: ini

            initializer = tortoise.contrib.test.env_initializer
            finalizer = tortoise.contrib.test.finalizer

        然后定义 ``TORTOISE_TEST_MODULES`` 环境变量，包含以逗号分隔的模块路径列表。

        此外，您可以将数据库配置参数设置为环境变量（默认为 ``sqlite://:memory:``）：

            TORTOISE_TEST_DB=sqlite:///tmp/test-{}.sqlite
            TORTOISE_TEST_DB=postgres://postgres:@127.0.0.1:5432/test_{}

    .. md-tab-item:: 英文

        In your ``.green`` file:

        .. code-block:: ini

            initializer = tortoise.contrib.test.env_initializer
            finalizer = tortoise.contrib.test.finalizer

        And then define the ``TORTOISE_TEST_MODULES`` environment variable with a comma separated list of module paths.

        Furthermore, you may set the database configuration parameter as an environment variable (defaults to ``sqlite://:memory:``):

            TORTOISE_TEST_DB=sqlite:///tmp/test-{}.sqlite
            TORTOISE_TEST_DB=postgres://postgres:@127.0.0.1:5432/test_{}


Py.test
-------

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. note::

            pytest 5.4.0 和 5.4.1 存在一个错误，导致它无法与异步测试用例一起工作。您可能需要安装 ``pytest>=5.4.2`` 才能使其正常工作。

        在您的 ``conftest.py`` 文件中运行初始化器和最终化器：

        .. code-block:: python3

            import os
            import pytest
            from tortoise.contrib.test import finalizer, initializer

            @pytest.fixture(scope="session", autouse=True)
            def initialize_tests(request):
                db_url = os.environ.get("TORTOISE_TEST_DB", "sqlite://:memory:")
                initializer(["tests.testmodels"], db_url=db_url, app_label="models")
                request.addfinalizer(finalizer)

    .. md-tab-item:: 英文

        .. note::

            pytest 5.4.0 & 5.4.1 has a bug that stops it from working with async test cases. You may have to install ``pytest>=5.4.2`` to get it to work.

        Run the initializer and finalizer in your ``conftest.py`` file:

        .. code-block:: python3

            import os
            import pytest
            from tortoise.contrib.test import finalizer, initializer

            @pytest.fixture(scope="session", autouse=True)
            def initialize_tests(request):
                db_url = os.environ.get("TORTOISE_TEST_DB", "sqlite://:memory:")
                initializer(["tests.testmodels"], db_url=db_url, app_label="models")
                request.addfinalizer(finalizer)


Nose2
-----

.. md-tab-set::
    
    .. md-tab-item:: 中文

        加载插件 ``tortoise.contrib.test.nose2`` 可以通过命令行执行：

            nose2 --plugin tortoise.contrib.test.nose2 --db-module tortoise.tests.testmodels

        或者通过配置文件：

        .. code-block:: ini

            [unittest]
            plugins = tortoise.contrib.test.nose2

            [tortoise]
            # 必须至少指定一个模块路径
            db-module =
                tests.testmodels
            # 您可以在这里可选地覆盖 db_url
            db-url = sqlite://testdb-{}.sqlite
    
    .. md-tab-item:: 英文

        Load the plugin ``tortoise.contrib.test.nose2`` either via command line::

            nose2 --plugin tortoise.contrib.test.nose2 --db-module tortoise.tests.testmodels

        Or via the config file:

        .. code-block:: ini

            [unittest]
            plugins = tortoise.contrib.test.nose2

            [tortoise]
            # Must specify at least one module path
            db-module =
                tests.testmodels
            # You can optionally override the db_url here
            db-url = sqlite://testdb-{}.sqlite


参考
=========

**Reference**

.. automodule:: tortoise.contrib.test
    :members:
    :undoc-members:
    :show-inheritance:
