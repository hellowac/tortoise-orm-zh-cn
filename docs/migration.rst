:tocdepth: 4

.. _migration:

=========
迁移
=========

**Migration**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
        本文档描述了如何使用 `Aerich` 进行迁移。

        您可以查看 `https://github.com/tortoise/aerich <https://github.com/tortoise/aerich>`_ 以获取更多详细信息。

    .. md-tab-item:: 英文

        This document describes how to use `Aerich` to make migrations.

        You can see `https://github.com/tortoise/aerich <https://github.com/tortoise/aerich>`_ for more details.

快速开始
===========

**Quick Start**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich -h

            用法： aerich [选项] 命令 [参数]...

            选项：
            -c, --config TEXT  配置文件。  [默认: aerich.ini]
            --app TEXT         Tortoise-ORM 应用名称。  [默认: models]
            -n, --name TEXT    用于 aerich 配置的 .ini 文件中节的名称。
                                [默认: aerich]
            -h, --help         显示此消息并退出。

            命令：
            downgrade  降级到指定版本。
            heads      显示迁移位置当前可用的头部。
            history    列出所有迁移项。
            init       初始化配置文件并生成根迁移位置。
            init-db    生成模式并生成应用迁移位置。
            migrate    生成迁移变更文件。
            upgrade    升级到最新版本。

    .. md-tab-item:: 英文

        .. code-block:: shell

            > aerich -h

            Usage: aerich [OPTIONS] COMMAND [ARGS]...

            Options:
            -c, --config TEXT  Config file.  [default: aerich.ini]
            --app TEXT         Tortoise-ORM app name.  [default: models]
            -n, --name TEXT    Name of section in .ini file to use for aerich config.
                                [default: aerich]
            -h, --help         Show this message and exit.

            Commands:
            downgrade  Downgrade to specified version.
            heads      Show current available heads in migrate location.
            history    List all migrate items.
            init       Init config file and generate root migrate location.
            init-db    Generate schema and generate app migrate location.
            migrate    Generate migrate changes file.
            upgrade    Upgrade to latest version.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        您需要先将 `aerich.models` 添加到您的 `Tortoise-ORM` 配置中，例如：
    
    .. md-tab-item:: 英文

        You need add `aerich.models` to your `Tortoise-ORM` config first, example:

.. code-block:: python3

    TORTOISE_ORM = {
        "connections": {"default": "mysql://root:123456@127.0.0.1:3306/test"},
        "apps": {
            "models": {
                "models": ["tests.models", "aerich.models"],
                "default_connection": "default",
            },
        },
    }

初始化
--------------

**Initialization**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich init -h

            用法： aerich init [选项]

            初始化配置文件并生成根迁移位置。

            选项：
            -t, --tortoise-orm TEXT  Tortoise-ORM 配置模块字典变量，例如 settings.TORTOISE_ORM。
                                    [必需]
            --location TEXT          迁移存储位置。  [默认: ./migrations]
            -h, --help               显示此消息并退出。

        初始化配置文件和位置：
    
    .. md-tab-item:: 英文

        .. code-block:: shell

            > aerich init -h

            Usage: aerich init [OPTIONS]

            Init config file and generate root migrate location.

            Options:
            -t, --tortoise-orm TEXT  Tortoise-ORM config module dict variable, like settings.TORTOISE_ORM.
                                    [required]
            --location TEXT          Migrate store location.  [default: ./migrations]
            -h, --help               Show this message and exit.


        Init config file and location:

.. code-block:: shell

    > aerich init -t tests.backends.mysql.TORTOISE_ORM

    Success create migrate location ./migrations
    Success generate config file aerich.ini


初始化db
-------

**Init db**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich init-db

            成功创建应用迁移位置 ./migrations/models
            成功为应用 "models" 生成架构(schema)


        如果您的 Tortoise-ORM 应用不是默认的 `models`，您必须指定 `--app`，例如 `aerich --app other_models init-db`。
    
    .. md-tab-item:: 英文

        .. code-block:: shell

            > aerich init-db

            Success create app migrate location ./migrations/models
            Success generate schema for app "models"


        If your Tortoise-ORM app is not default `models`, you must specify `--app` like `aerich --app other_models init-db`.

更新模型并生成迁移文件
------------------------------

**Update models and make migrate**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich migrate --name drop_column

            成功迁移 1_202029051520102929_drop_column.json


        迁移文件名的格式为 `{version_num}_{datetime}_{name|update}.json`。

        如果 `aerich` 认为您正在重命名列，它会询问 `Rename {old_column} to {new_column} [True]`，您可以选择 `True` 以在不删除列的情况下重命名，或选择 `False` 以先删除列再创建。

        如果您使用 `MySQL`，只有 MySQL8.0+ 支持 `rename..to` 语法。
    
    .. md-tab-item:: 英文

        ..  code-block:: shell

            > aerich migrate --name drop_column

            Success migrate 1_202029051520102929_drop_column.json


        Format of migrate filename is
        `{version_num}_{datetime}_{name|update}.json`.

        And if `aerich` guess you are renaming a column, it will ask `Rename {old_column} to {new_column} [True]`, you can choice `True` to rename column without column drop, or choice `False` to drop column then create.

        If you use `MySQL`, only MySQL8.0+ support `rename..to` syntax.

升级到最新版本
-------------------------

**Upgrade to latest version**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich upgrade

            Success upgrade 1_202029051520102929_drop_column.json

        现在你的数据已经迁移到最新版本。
    
    .. md-tab-item:: 英文

        .. code-block:: shell

            > aerich upgrade

            Success upgrade 1_202029051520102929_drop_column.json

        Now your db is migrated to latest.

降级到指定版本
------------------------------

**Downgrade to specified version**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > aerich init -h

            Usage: aerich downgrade [OPTIONS]

            Downgrade to specified version.

            Options:
            -v, --version INTEGER  Specified version, default to last.  [default: -1]
            -h, --help             Show this message and exit.

        .. code-block:: shell

            > aerich downgrade

            Success downgrade 1_202029051520102929_drop_column.json

        现在你的数据库回滚到指定版本.
    
    .. md-tab-item:: 英文

        .. code-block:: shell

            > aerich init -h

            Usage: aerich downgrade [OPTIONS]

            Downgrade to specified version.

            Options:
            -v, --version INTEGER  Specified version, default to last.  [default: -1]
            -h, --help             Show this message and exit.

        .. code-block:: shell

            > aerich downgrade

            Success downgrade 1_202029051520102929_drop_column.json


        Now your db rollback to specified version.

显示历史
------------

**Show history**

.. code-block:: shell

    > aerich history

    1_202029051520102929_drop_column.json


显示已迁移的头(header)
-------------------------

**Show heads to be migrated**

.. md-tab-set::
    
    .. md-tab-item:: 中文
    
    .. md-tab-item:: 英文

.. code-block:: shell

    > aerich heads

    1_202029051520102929_drop_column.json

