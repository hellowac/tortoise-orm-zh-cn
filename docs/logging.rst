:tocdepth: 4

=======
Logging
=======

.. md-tab-set::
    
    .. md-tab-item:: 中文

        当前的 Tortoise 有两个日志记录器， `tortoise.db_client` 和 `tortoise`。

        `tortoise.db_client` 记录执行查询的信息，而 `tortoise` 记录运行时的信息。

        如果您想控制 Tortoise 日志的行为，例如打印调试 SQL，您可以按照以下方式进行配置。

        .. code-block:: python3

            import logging
            import sys

            fmt = logging.Formatter(
                fmt="%(asctime)s - %(name)s:%(lineno)d - %(levelname)s - %(message)s",
                datefmt="%Y-%m-%d %H:%M:%S",
            )
            sh = logging.StreamHandler(sys.stdout)
            sh.setLevel(logging.DEBUG)
            sh.setFormatter(fmt)

            # 将打印调试 SQL
            logger_db_client = logging.getLogger("tortoise.db_client")
            logger_db_client.setLevel(logging.DEBUG)
            logger_db_client.addHandler(sh)

            logger_tortoise = logging.getLogger("tortoise")
            logger_tortoise.setLevel(logging.DEBUG)
            logger_tortoise.addHandler(sh)


        您还可以使用自己的格式化程序为 SQL 添加语法高亮，通过使用 `pygments <https://pygments.org/>`：

        .. code-block:: python3

            import logging

            from pygments import highlight
            from pygments.formatters.terminal import TerminalFormatter
            from pygments.lexers.sql import PostgresLexer

            postgres = PostgresLexer()
            terminal_formatter = TerminalFormatter()


            class PygmentsFormatter(logging.Formatter):
                def __init__(
                    self,
                    fmt="{asctime} - {name}:{lineno} - {levelname} - {message}",
                    datefmt="%H:%M:%S",
                ):
                    self.datefmt = datefmt
                    self.fmt = fmt
                    logging.Formatter.__init__(self, None, datefmt)

                def format(self, record: logging.LogRecord):
                    """使用 SQL 的语法高亮格式化日志记录。"""
                    own_records = {
                        attr: val
                        for attr, val in record.__dict__.items()
                        if not attr.startswith("_")
                    }
                    message = record.getMessage()
                    name = record.name
                    asctime = self.formatTime(record, self.datefmt)

                    if name == "tortoise.db_client":
                        if (
                            record.levelname == "DEBUG"
                            and not message.startswith("Created connection pool")
                            and not message.startswith("Closed connection pool")
                        ):
                            message = highlight(message, postgres, terminal_formatter).rstrip()

                    own_records.update(
                        {
                            "message": message,
                            "name": name,
                            "asctime": asctime,
                        }
                    )

                    return self.fmt.format(**own_records)



            # 然后将上面的格式化程序替换为以下格式化程序
            fmt = PygmentsFormatter(
                fmt="{asctime} - {name}:{lineno} - {levelname} - {message}",
                datefmt="%Y-%m-%d %H:%M:%S",
            )

    .. md-tab-item:: 英文

        Current tortoise has two loggers, `tortoise.db_client` and `tortoise`.

        `tortoise.db_client` logging the information about execute query, and `tortoise` logging the information about runtime.

        If you want control the behavior of tortoise logging, such as print debug sql, you can configure it yourself like following.

        .. code-block:: python3

            import logging

            fmt = logging.Formatter(
                fmt="%(asctime)s - %(name)s:%(lineno)d - %(levelname)s - %(message)s",
                datefmt="%Y-%m-%d %H:%M:%S",
            )
            sh = logging.StreamHandler(sys.stdout)
            sh.setLevel(logging.DEBUG)
            sh.setFormatter(fmt)

            # will print debug sql
            logger_db_client = logging.getLogger("tortoise.db_client")
            logger_db_client.setLevel(logging.DEBUG)
            logger_db_client.addHandler(sh)

            logger_tortoise = logging.getLogger("tortoise")
            logger_tortoise.setLevel(logging.DEBUG)
            logger_tortoise.addHandler(sh)


        You can also use your own formatter to add syntax coloration to sql, by using `pygments <https://pygments.org/>` :

        .. code-block:: python3

            import logging

            from pygments import highlight
            from pygments.formatters.terminal import TerminalFormatter
            from pygments.lexers.sql import PostgresLexer

            postgres = PostgresLexer()
            terminal_formatter = TerminalFormatter()


            class PygmentsFormatter(logging.Formatter):
                def __init__(
                    self,
                    fmt="{asctime} - {name}:{lineno} - {levelname} - {message}",
                    datefmt="%H:%M:%S",
                ):
                    self.datefmt = datefmt
                    self.fmt = fmt
                    logging.Formatter.__init__(self, None, datefmt)

                def format(self, record: logging.LogRecord):
                    """Format the logging record with slq's syntax coloration."""
                    own_records = {
                        attr: val
                        for attr, val in record.__dict__.items()
                        if not attr.startswith("_")
                    }
                    message = record.getMessage()
                    name = record.name
                    asctime = self.formatTime(record, self.datefmt)

                    if name == "tortoise.db_client":
                        if (
                            record.levelname == "DEBUG"
                            and not message.startswith("Created connection pool")
                            and not message.startswith("Closed connection pool")
                        ):
                            message = highlight(message, postgres, terminal_formatter).rstrip()

                    own_records.update(
                        {
                            "message": message,
                            "name": name,
                            "asctime": asctime,
                        }
                    )

                    return self.fmt.format(**own_records)



            # Then replace the formatter above by the following one
            fmt = PygmentsFormatter(
                fmt="{asctime} - {name}:{lineno} - {levelname} - {message}",
                datefmt="%Y-%m-%d %H:%M:%S",
            )
