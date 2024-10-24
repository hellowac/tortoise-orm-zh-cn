:tocdepth: 4

.. _manager:

=======
Manager
=======

.. md-tab-set::
    
    .. md-tab-item:: 中文

        Manager 是提供数据库查询操作的接口，通过它可以与 Tortoise 模型进行交互。

        每个 Tortoise 模型都有一个默认的 Manager。

    .. md-tab-item:: 英文

        A Manager is the interface through which database query operations are provided to tortoise models.

        There is one default Manager for every tortoise model.

用法
=====

**Usage**

.. md-tab-set::
    
    .. md-tab-item:: 中文

        使用管理器有两种方式，一种是在 `Meta` 中使用 `manager` 来覆盖默认的管理器，另一种是在模型中定义管理器：

        .. code-block:: python3

            from tortoise.manager import Manager

            class StatusManager(Manager):
                def get_queryset(self):
                    return super(StatusManager, self).get_queryset().filter(status=1)


            class ManagerModel(Model):
                status = fields.IntField(default=0)
                all_objects = Manager()

                class Meta:
                    manager = StatusManager()


        在覆盖默认管理器后，所有像 `Model.get()`、`Model.filter()` 的查询都会遵循自定义管理器的行为。

        在上面的例子中，使用默认管理器时，你无法获取状态等于 `0` 的对象，但可以使用在模型中定义的管理器 `all_objects` 来获取所有对象。

    .. md-tab-item:: 英文

        There are two ways to use a Manager, one is use `manager` in `Meta` to override the default `manager`, another is define manager in model:

        .. code-block:: python3

            from tortoise.manager import Manager

            class StatusManager(Manager):
                def get_queryset(self):
                    return super(StatusManager, self).get_queryset().filter(status=1)


            class ManagerModel(Model):
                status = fields.IntField(default=0)
                all_objects = Manager()

                class Meta:
                    manager = StatusManager()


        After override default manager, all queries like `Model.get()`, `Model.filter()` will be comply with the behavior of custom manager.

        In the example above, you can never get the objects which status is equal to `0` with default manager, but you can use the manager `all_objects` defined in model to get all objects.

.. code-block:: python3

    m1 = await ManagerModel.create()
    m2 = await ManagerModel.create(status=1)

    self.assertEqual(await ManagerModel.all().count(), 1)
    self.assertEqual(await ManagerModel.all_objects.count(), 2)

    self.assertIsNone(await ManagerModel.get_or_none(pk=m1.pk))
    self.assertIsNotNone(await ManagerModel.all_objects.get_or_none(pk=m1.pk))
    self.assertIsNotNone(await ManagerModel.get_or_none(pk=m2.pk))
