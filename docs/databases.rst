:tocdepth: 3

.. _databases:

=========
数据库
=========

**Databases**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise 目前支持以下数据库:

        * SQLite(使用 ``aiosqlite`` )
        * PostgreSQL >= 9.4(使用 ``asyncpg`` 或 ``psycopg`` )
        * MySQL/MariaDB(使用 ``asyncmy`` 或 ``aiomysql`` )
        * Microsoft SQL Server(使用 ``asyncodbc`` )

        使用时，请确保已安装相应的 asyncio 驱动程序。
    
    .. md-tab-item:: 英文

        Tortoise currently supports the following databases:

        * SQLite (using ``aiosqlite`` )
        * PostgreSQL >= 9.4 (using ``asyncpg`` or ``psycopg`` )
        * MySQL/MariaDB (using ``asyncmy`` or ``aiomysql`` )
        * Microsoft SQL Server (using ``asyncodbc`` )

        To use, please ensure that corresponding asyncio driver is installed.

.. _db_url:

DB_URL
======

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Tortoise 支持以 URL 形式指定数据库配置。

        格式为:

        :samp:`{DB_TYPE}://{USERNAME}:{PASSWORD}@{HOST}:{PORT}/{DB_NAME}?{PARAM1}=value&{PARAM2}=value`

        如果密码包含特殊字符，则需要进行 URL 编码:

        .. code-block:: python3

            >>> import urllib.parse
            >>> urllib.parse.quote_plus("kx%jj5/g")
            'kx%25jj5%2Fg'

        支持的 ``DB_TYPE``:

        ``sqlite``:
            通常格式为 :samp:`sqlite://{DB_FILE}`
            如果 ``DB_FILE`` 是 "/data/db.sqlite3"，则字符串为 ``sqlite:///data/db.sqlite3`` (注意三个 /)

        ``postgres``:
            使用 ``asyncpg``:
            通常格式为 :samp:`postgres://postgres:pass@db.host:5432/somedb`

            或使用具体的 ``asyncpg``/``psycopg``:

            - ``psycopg``: :samp:`psycopg://postgres:pass@db.host:5432/somedb`
            - ``asyncpg``: :samp:`asyncpg://postgres:pass@db.host:5432/somedb`

        ``mysql``:
            通常格式为 :samp:`mysql://myuser:mypass@db.host:3306/somedb`

        ``mssql``:
            通常格式为 :samp:`mssql://myuser:mypass@db.host:1433/somedb?driver=the odbc driver`
    
    .. md-tab-item:: 英文

        Tortoise supports specifying Database configuration in a URL form.

        The form is:

        :samp:`{DB_TYPE}://{USERNAME}:{PASSWORD}@{HOST}:{PORT}/{DB_NAME}?{PARAM1}=value&{PARAM2}=value`

        If password contains special characters it need to be URL encoded:

        .. code-block::  python3

            >>> import urllib.parse
            >>> urllib.parse.quote_plus("kx%jj5/g")
            'kx%25jj5%2Fg'

        The supported ``DB_TYPE``:

        ``sqlite``:
            Typically in the form of :samp:`sqlite://{DB_FILE}`
            So if the ``DB_FILE`` is "/data/db.sqlite3" then the string will be ``sqlite:///data/db.sqlite`` (note the three /'s)
        ``postgres``
            Using ``asyncpg``:
            Typically in the form of :samp:`postgres://postgres:pass@db.host:5432/somedb`

            Or specifically ``asyncpg``/``psycopg`` using:

            - ``psycopg``: :samp:`psycopg://postgres:pass@db.host:5432/somedb`
            - ``asyncpg``: :samp:`asyncpg://postgres:pass@db.host:5432/somedb`
        ``mysql``:
            Typically in the form of :samp:`mysql://myuser:mypass@db.host:3306/somedb`
        ``mssql``:
            Typically in the form of :samp:`mssql://myuser:mypass@db.host:1433/somedb?driver=the odbc driver`

功能
============

**Capabilities**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        由于每个数据库具有不同的功能集，我们在每个客户端上注册了 ``Capabilities``。
        主要目的是解决 SQL 之间的重大差异或常见问题。

    .. md-tab-item:: 英文

        Since each database has a different set of features we have a ``Capabilities`` that is registered on each client.
        Primarily this is to work around larger-than SQL differences, or common issues.

.. autoclass:: tortoise.backends.base.client.Capabilities
    :members:


SQLite
======

.. md-tab-set::
    
    .. md-tab-item:: 中文
        
        SQLite 是一个嵌入式数据库，可以运行在文件或内存中。适合用于本地开发或代码逻辑的测试，但不推荐用于生产环境。

        .. caution::

            SQLite 并不原生支持许多常见的数据类型，虽然我们在可能的情况下进行了仿真，但并不是所有的都完美。

            例如，``DecimalField`` 通过将值存储为字符串来保持精度，但在进行聚合/排序时，必须在浮点数之间进行转换。

            同样，大小写不敏感的实现也仅部分完成。

        数据库 URL 通常格式为 :samp:`sqlite://{DB_FILE}`，因此如果 ``DB_FILE`` 是 "/data/db.sqlite3"，则字符串为 ``sqlite:///data/db.sqlite3`` (注意三个 /)。

    .. md-tab-item:: 英文

        SQLite is an embedded database, and can run on a file or in-memory. Good database for local development or testing of code logic, but not recommended for production use.

        .. caution::

            SQLite doesn't support many of the common datatypes natively, although we do emulation where we can, not everything is perfect.

            For example ``DecimalField`` has precision preserved by storing values as strings, except when doing aggregates/ordering on it. In those cases we have to cast to/from floating-point numbers.

            Similarly case-insensitivity is only partially implemented.

        DB URL is typically in the form of :samp:`sqlite://{DB_FILE}` So if the ``DB_FILE`` is "/data/db.sqlite3" then the string will be ``sqlite:///data/db.sqlite`` (note the three /'s)

必选参数
-------------------

**Required Parameters**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        ``path``:

            指向 SQLite3 文件的路径。``:memory:`` 是一个特殊路径，表示使用内存数据库。

    .. md-tab-item:: 英文

        ``path``:

            Path to SQLite3 file. ``:memory:`` is a special path that indicates in-memory database.

可选参数
--------------------

**Optional parameters:**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        SQLite 可选参数基本上是任何文档中记录的 ``PRAGMA`` 语句 `here. <https://sqlite.org/pragma.html#toc>`__

        ``journal_mode`` (默认为 ``WAL`` ):
            指定 SQLite 日志模式。
        ``journal_size_limit`` (默认为 ``16384`` ):
            日志大小。
        ``foreign_keys``  (默认为 ``ON`` ):
            设置为 ``OFF`` 以不强制执行引用完整性。
            
    .. md-tab-item:: 英文

        SQLite optional parameters is basically any of the ``PRAGMA`` statements documented `here. <https://sqlite.org/pragma.html#toc>`__

        ``journal_mode`` (defaults to ``WAL`` ):
            Specify SQLite journal mode.
        ``journal_size_limit`` (defaults to ``16384`` ):
            The journal size.
        ``foreign_keys``  (defaults to ``ON`` )
            Set to ``OFF`` to not enforce referential integrity.


PostgreSQL
==========

.. md-tab-set::
    
    .. md-tab-item:: 中文

        数据库 URL 通常格式为 :samp:`postgres://postgres:pass@db.host:5432/somedb` ，或者，如果通过 Unix 域套接字连接，则为 :samp:`postgres:///somedb` 。
    
    .. md-tab-item:: 英文

        DB URL is typically in the form of :samp:`postgres://postgres:pass@db.host:5432/somedb`, or, if connecting via Unix domain socket :samp:`postgres:///somedb`.

必选参数
-------------------

**Required Parameters**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        ``user``:
            连接时使用的用户名。
        ``password``:
            用户名的密码。
        ``host``:
            数据库可用的网络主机。
        ``port``:
            数据库可用的网络端口。(默认为 ``5432`` )
        ``database``:
            要使用的数据库。

    .. md-tab-item:: 英文

        ``user``:
            Username to connect with.
        ``password``:
            Password for username.
        ``host``:
            Network host that database is available at.
        ``port``:
            Network port that database is available at. (defaults to ``5432`` )
        ``database``:
            Database to use.

可选参数
--------------------

**Optional parameters:**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        PostgreSQL 可选参数是直接传递给驱动程序的参数，更多细节请参见 `这里 <https://magicstack.github.io/asyncpg/current/api/index.html#connection-pools>`__。

        ``minsize`` (默认为 ``1`` ):
            连接池的最小大小
        ``maxsize`` (默认为 ``5`` ):
            连接池的最大大小
        ``max_queries`` (默认为 ``50000`` ):
            在关闭并替换连接之前的最大查询次数。
        ``max_inactive_connection_lifetime`` (默认为 ``300.0`` ):
            在假定连接已失效并强制重新连接之前的非活动连接持续时间。
        ``schema`` (默认使用用户的默认模式):
            默认使用的特定模式。
        ``ssl`` (默认为 ``False`` ):
            可以是 ``True`` 或用于自签名证书的自定义 SSL 上下文。有关更多信息，请参见 :ref:`db_ssl`。

        如果缺少 ``user``、``password``、``host``、``port`` 参数，我们将让 ``asyncpg``/``psycopg`` 从默认来源(标准 PostgreSQL 环境变量或默认值)中检索。

    .. md-tab-item:: 英文

        PostgreSQL optional parameters are pass-though parameters to the driver, see `here <https://magicstack.github.io/asyncpg/current/api/index.html#connection-pools>`__ for more details.

        ``minsize`` (defaults to ``1`` ):
            Minimum connection pool size
        ``maxsize`` (defaults to ``5`` ):
            Maximum connection pool size
        ``max_queries`` (defaults to ``50000`` ):
            Maximum no of queries before a connection is closed and replaced.
        ``max_inactive_connection_lifetime`` (defaults to ``300.0`` ):
            Duration of inactive connection before assuming that it has gone stale, and force a re-connect.
        ``schema`` (uses user's default schema by default):
            A specific schema to use by default.
        ``ssl`` (defaults to ''False`` ):
            Either ``True`` or a custom SSL context for self-signed certificates. See :ref:`db_ssl` for more info.

        In case any of ``user``, ``password``, ``host``, ``port`` parameters is missing, we are letting ``asyncpg``/``psycopg`` retrieve it from default sources (standard PostgreSQL environment variables or default values).


MySQL/MariaDB
=============

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        DB URL 通常采用以下形式: :samp:`mysql://myuser:mypass@db.host:3306/somedb`

    .. md-tab-item:: 英文

        DB URL is typically in the form of :samp:`mysql://myuser:mypass@db.host:3306/somedb`

必选参数
-------------------

**Required Parameters**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        ``user``:
            连接所用的用户名。
        ``password``:
            用户名的密码。
        ``host``:
            数据库可用的网络主机。
        ``port``:
            数据库可用的网络端口。(默认为 ``3306`` )
        ``database``:
            要使用的数据库。
    
    .. md-tab-item:: 英文

        ``user``:
            Username to connect with.
        ``password``:
            Password for username.
        ``host``:
            Network host that database is available at.
        ``port``:
            Network port that database is available at. (defaults to ``3306`` )
        ``database``:
            Database to use.

可选参数
--------------------

**Optional parameters:**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        MySQL 可选参数是传递给驱动程序的参数，更多细节请参见 `here <https://aiomysql.readthedocs.io/en/latest/connection.html#connection>`__。

        ``minsize`` (默认为 ``1`` ):
            最小连接池大小
        ``maxsize`` (默认为 ``5`` ):
            最大连接池大小
        ``connect_timeout`` (默认为 ``None`` ):
            在抛出错误之前等待连接的持续时间。
        ``echo`` (默认为 ``False`` ):
            设置为 ``True`` 以回显 SQL 查询(仅用于调试)
        ``charset`` (默认为 ``utf8mb4`` ):
            设置使用的字符集
        ``ssl`` (默认为 ``False`` ):
            可以是 ``True`` 或自定义 SSL 上下文，用于自签名证书。有关更多信息，请参见 :ref:`db_ssl`。
    
    .. md-tab-item:: 英文

        MySQL optional parameters are pass-though parameters to the driver, see `here <https://aiomysql.readthedocs.io/en/latest/connection.html#connection>`__ for more details.

        ``minsize`` (defaults to ``1`` ):
            Minimum connection pool size
        ``maxsize`` (defaults to ``5`` ):
            Maximum connection pool size
        ``connect_timeout`` (defaults to ``None`` ):
            Duration to wait for connection before throwing error.
        ``echo`` (defaults to ``False`` ):
            Set to `True`` to echo SQL queries (debug only)
        ``charset`` (defaults to ``utf8mb4`` ):
            Sets the character set in use
        ``ssl`` (defaults to ``False`` ):
            Either ``True`` or a custom SSL context for self-signed certificates. See :ref:`db_ssl` for more info.

.. _db_ssl:

MSSQL/Oracle
============

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        数据库 URL 通常的形式为 :samp:`mssql 或 oracle://myuser:mypass@db.host:1433/somedb?driver={the odbc driver}` 。

    .. md-tab-item:: 英文

        DB URL is typically in the form of :samp:`mssql or oracle://myuser:mypass@db.host:1433/somedb?driver={the odbc driver}`

必须参数
-------------------

**Required Parameters**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        ``user``:
            用于连接的用户名。
        ``password``:
            用户名的密码。
        ``host``:
            数据库可用的网络主机。
        ``port``:
            数据库可用的网络端口。(默认为 ``1433`` )
        ``database``:
            要使用的数据库。
        ``driver``:
            要使用的 ODBC 驱动程序。在您的 `odbcinst.ini` 文件中 ODBC 驱动程序的实际名称(您可以使用 `odbcinst -j` 命令找到其位置)。需要在系统中安装 `unixodbc`。
    
    .. md-tab-item:: 英文

        ``user``:
            Username to connect with.
        ``password``:
            Password for username.
        ``host``:
            Network host that database is available at.
        ``port``:
            Network port that database is available at. (defaults to ``1433`` )
        ``database``:
            Database to use.
        ``driver``:
            The ODBC driver to use. Actual name of the ODBC driver in your `odbcinst.ini` file (you can find it's location using `odbcinst -j` command). It requires `unixodbc` to be installed in your system.

可选参数
--------------------

**Optional parameters:**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        MSSQL/Oracle 可选参数是传递给驱动程序的参数，详情见 `here <https://github.com/tortoise/asyncodbc>`__。

        ``minsize`` (默认为 ``1`` ):
            最小连接池大小
        ``maxsize`` (默认为 ``10`` ):
            最大连接池大小
        ``pool_recycle`` (默认为 ``-1`` ):
            连接池回收超时时间(秒)。
        ``echo`` (默认为 ``False`` ):
            设置为 ``True`` 以回显 SQL 查询(仅用于调试)。
    
    .. md-tab-item:: 英文

        MSSQL/Oracle optional parameters are pass-though parameters to the driver, see `here <https://github.com/tortoise/asyncodbc>`__ for more details.

        ``minsize`` (defaults to ``1`` ):
            Minimum connection pool size
        ``maxsize`` (defaults to ``10`` ):
            Maximum connection pool size
        ``pool_recycle`` (defaults to ``-1`` ):
            Pool recycle timeout in seconds.
        ``echo`` (defaults to ``False`` ):
            Set to ``True`` to echo SQL queries (debug only)

Oracle中的编码
========================

**Encoding in Oracle:**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        如果在 Varchar 字段中看到 ``???`` 值而不是实际文本(如俄文/中文等)，请在客户端环境中设置 ``NLS_LANG`` 变量以支持 UTF8。例如，设置为 `"American_America.UTF8"`。
    
    .. md-tab-item:: 英文

        If you get ``???`` values in Varchar fields instead of your actual text (russian/chinese/etc), then set ``NLS_LANG`` variable in your client environment to support UTF8. For example, `"American_America.UTF8"`.


传递自定义SSL证书
==================================

**Passing in custom SSL Certificates**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        要传递自定义 SSL 证书，需要使用详细的初始化结构，因为 URL 解析器无法处理复杂对象。

        .. code-block:: python3

            # 这里我们创建一个自定义的 SSL 上下文
            import ssl
            ctx = ssl.create_default_context()
            # 在这个示例中，我们禁用验证...
            # 请不要这样做。请查看官方 Python ``ssl`` 模块文档
            ctx.check_hostname = False
            ctx.verify_mode = ssl.CERT_NONE

            # 这里我们进行详细初始化
            await Tortoise.init(
                config={
                    "connections": {
                        "default": {
                            "engine": "tortoise.backends.asyncpg",
                            "credentials": {
                                "database": None,
                                "host": "127.0.0.1",
                                "password": "moo",
                                "port": 54321,
                                "user": "postgres",
                                "ssl": ctx  # 在这里传递 SSL 上下文
                            }
                        }
                    },
                    "apps": {
                        "models": {
                            "models": ["some.models"],
                            "default_connection": "default",
                        }
                    },
                }
            )
    
    .. md-tab-item:: 英文

        To pass in a custom SSL Cert, one has to use the verbose init structure as the URL parser can't
        handle complex objects.

        .. code-block::  python3

            # Here we create a custom SSL context
            import ssl
            ctx = ssl.create_default_context()
            # And in this example we disable validation...
            # Please don't do this. Look at the official Python ``ssl`` module documentation
            ctx.check_hostname = False
            ctx.verify_mode = ssl.CERT_NONE

            # Here we do a verbose init
            await Tortoise.init(
                config={
                    "connections": {
                        "default": {
                            "engine": "tortoise.backends.asyncpg",
                            "credentials": {
                                "database": None,
                                "host": "127.0.0.1",
                                "password": "moo",
                                "port": 54321,
                                "user": "postgres",
                                "ssl": ctx  # Here we pass in the SSL context
                            }
                        }
                    },
                    "apps": {
                        "models": {
                            "models": ["some.models"],
                            "default_connection": "default",
                        }
                    },
                }
            )

.. _base_db_client:

基本DB客户端
==============

**Base DB client**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        基础数据库客户端接口在此提供，但应仅在高级情况下直接使用。
    
    .. md-tab-item:: 英文

        The Base DB client interface is provided here, but should only be directly used as an advanced case.

.. automodule:: tortoise.backends.base.client

    .. autoclass:: BaseDBAsyncClient
        :members:
        :exclude-members: query_class, executor_class, schema_generator, capabilities
