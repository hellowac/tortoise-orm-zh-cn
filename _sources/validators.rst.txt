:tocdepth: 3

.. _validators:

==========
校验
==========

**Validators**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        验证器是模型字段的可调用函数，它接受一个值，如果不符合某些条件则引发“ValidationError”。
    
    .. md-tab-item:: 英文

        A validator is a callable for model field that takes a value and raises a `ValidationError` if it doesn’t meet some criteria.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        您可以将验证器列表传递给 `Field` 的 `validators` 参数：

        .. code-block:: python3

            class ValidatorModel(Model):
                regex = fields.CharField(max_length=100, null=True, validators=[RegexValidator("abc.+", re.I)])

            # 哦不，这将引发 ValidationError！
            await ValidatorModel.create(regex="ccc")
            # 这很好！
            await ValidatorModel.create(regex="abcd")
    
    .. md-tab-item:: 英文

        You can pass a list of validators to `Field` parameter `validators`:

        .. code-block:: python3

            class ValidatorModel(Model):
                regex = fields.CharField(max_length=100, null=True, validators=[RegexValidator("abc.+", re.I)])

            # oh no, this will raise ValidationError!
            await ValidatorModel.create(regex="ccc")
            # this is great!
            await ValidatorModel.create(regex="abcd")

内置校验器
===================

**Built-in Validators**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        这儿是一个内置校验器的列表:
    
    .. md-tab-item:: 英文

        Here is the list of built-in validators:

.. automodule:: tortoise.validators
    :members:
    :undoc-members:

自定义校验器
================

**Custom Validator**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        有两种方法来编写自定义验证器，一种是通过传递给定值编写函数，另一种是继承 `tortoise.validators.Validator` 并实现 `__call__`。

        以下是编写自定义验证器的示例，用于验证给定值是否为偶数：

        .. code-block:: python3

            from tortoise.validators import Validator
            from tortoise.exceptions import ValidationError

            class EvenNumberValidator(Validator):
                """
                验证器，用于验证给定值是否为偶数。
                """
                def __call__(self, value: int):
                    if value % 2 != 0:
                        raise ValidationError(f"值 '{value}' 不是偶数")

            # 或者使用函数而不是类
            def validate_even_number(value: int):
                if value % 2 != 0:
                    raise ValidationError(f"值 '{value}' 不是偶数")
    
    .. md-tab-item:: 英文

        There are two methods to write a custom validator, one you can write a function by passing a given value, another you can inherit `tortoise.validators.Validator` and implement `__call__`.

        Here is a example to write a custom validator to validate the given value is an even number:

        .. code-block:: python3

            from tortoise.validators import Validator
            from tortoise.exceptions import ValidationError

            class EvenNumberValidator(Validator):
                """
                A validator to validate whether the given value is an even number or not.
                """
                def __call__(self, value: int):
                    if value % 2 != 0:
                        raise ValidationError(f"Value '{value}' is not an even number")

            # or use function instead of class
            def validate_even_number(value:int):
                if value % 2 != 0:
                    raise ValidationError(f"Value '{value}' is not an even number")
