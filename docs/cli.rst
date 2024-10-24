:tocdepth: 4

.. _cli:

===========
TortoiseCLI
===========

.. md-tab-set::
    
    .. md-tab-item:: 中文

        本文档介绍了如何使用 `tortoise-cli`，它是 tortoise-orm 的一个命令行工具，基于 click 和 ptpython 构建。

        您可以查看 `https://github.com/tortoise/tortoise-cli <https://github.com/tortoise/tortoise-cli>`_ 了解更多详细信息。
    
    .. md-tab-item:: 英文

        This document describes how to use `tortoise-cli`, a cli tool for tortoise-orm, build on top of click and ptpython.

        You can see `https://github.com/tortoise/tortoise-cli <https://github.com/tortoise/tortoise-cli>`_ for more details.


快速开始
===========

**Quick Start**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        .. code-block:: shell

            > tortoise-cli -h
            Usage: tortoise-cli [OPTIONS] COMMAND [ARGS]...

            Options:
            -V, --version      显示版本并退出。
            -c, --config TEXT  TortoiseORM 配置字典路径，例如 settings.TORTOISE_ORM
            -h, --help         显示此消息并退出。

            Commands:
            shell  启动一个交互式 shell。
    
    .. md-tab-item:: 英文

        .. code-block:: shell

            > tortoise-cli -h                                                                                                                                                                 23:59:38
            Usage: tortoise-cli [OPTIONS] COMMAND [ARGS]...

            Options:
            -V, --version      Show the version and exit.
            -c, --config TEXT  TortoiseORM config dictionary path, like settings.TORTOISE_ORM
            -h, --help         Show this message and exit.

            Commands:
            shell  Start an interactive shell.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        首先，您需要创建一个 TortoiseORM 配置对象，假设它在 `settings.py` 中。
    
    .. md-tab-item:: 英文

        First, you need make a TortoiseORM config object, assuming that in `settings.py`.

.. code-block:: shell

    TORTOISE_ORM = {
        "connections": {
            "default": "sqlite://:memory:",
        },
        "apps": {
            "models": {"models": ["examples.models"], "default_connection": "default"},
        },
    }


交互式 shell
=================

**Interactive shell**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        然后，您可以启动 TortoiseORM 的交互式 shell。

        .. code-block:: shell

            tortoise-cli -c settings.TORTOISE_ORM shell

        或者，您可以通过设置环境变量来设置配置。

        .. code-block:: shell

            export TORTOISE_ORM=settings.TORTOISE_ORM

        然后只需运行：

        .. code-block:: shell

            tortoise-cli shell
    
    .. md-tab-item:: 英文

        Then you can start an interactive shell for TortoiseORM.

        .. code-block:: shell

            tortoise-cli -c settings.TORTOISE_ORM shell


        Or you can set config by set environment variable.

        .. code-block:: shell

            export TORTOISE_ORM=settings.TORTOISE_ORM

        Then just run:

        .. code-block:: shell

            tortoise-cli shell
