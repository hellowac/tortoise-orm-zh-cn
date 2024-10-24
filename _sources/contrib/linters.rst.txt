=======
Linters
=======

.. _pylint:

PyLint 插件
=============

**PyLint plugin**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        由于 Tortoise ORM 使用 MetaClasses 来构建模型对象，PyLint 通常无法理解模型的行为。我们提供了一个 `tortoise.pylint` 插件，可以增强 PyLint 对模型和字段的理解。
    
    .. md-tab-item:: 英文

        Since Tortoise ORM uses MetaClasses to build the Model objects, PyLint will often not understand how the Models behave. We provided a `tortoise.pylint` plugin that enhances PyLints understanding of Models and Fields.

用法
-----

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        在项目的 ``.pylintrc`` 文件中，确保设置了以下内容：
    
    .. md-tab-item:: 英文

        In your projects ``.pylintrc`` file, ensure the following is set:

.. code-block:: ini

    load-plugins=tortoise.contrib.pylint


