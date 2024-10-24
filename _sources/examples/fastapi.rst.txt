.. _example_fastapi:

================
FastAPI 样例
================

**FastAPI Examples**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        这是 :ref:`contrib_fastapi` 的一个示例
    
    .. md-tab-item:: 英文

        This is an example of the  :ref:`contrib_fastapi`

**Usage:**

.. code-block:: sh

    uvicorn main:app --reload


.. rst-class:: emphasize-children

基本非关系示例
============================

**Basic non-relational example**

models.py
---------
.. literalinclude::  ../../examples/fastapi/models.py

tests.py
--------
.. literalinclude::  ../../examples/fastapi/_tests.py

main.py
-------
.. literalinclude::  ../../examples/fastapi/main.py
