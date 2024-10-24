.. _example_blacksheep:

===================
BlackSheep 样例
===================

**BlackSheep Examples**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        这是 :ref:`contrib_blacksheep` 的一个示例
    
    .. md-tab-item:: 英文

        This is an example of the  :ref:`contrib_blacksheep`

**Usage:**

.. code-block:: sh

    uvicorn server:app --reload


.. rst-class:: emphasize-children

基本非关系示例
============================

**Basic non-relational example**

models.py
---------
.. literalinclude::  ../../examples/blacksheep/models.py

test_api.py
--------
.. literalinclude::  ../../examples/blacksheep/test_api.py

server.py
-------
.. literalinclude::  ../../examples/blacksheep/server.py
