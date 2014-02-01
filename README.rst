Django-Cache-Magic
==================

Cache magic addresses two common scenarios for caching and cache invalidation
for django models: *instance caching* and *related objects caching*.

Instance caching: Storing instances of objects in your cache layer
as well as your database.

Related objects caching: Storing a collection of objects related to another
object referenced by relational constraints (ForeignKey, ManyToMany, etc.)

Installing
----------
.. code-block:: bash

    pip install django-cache-magic

Usage
-----
To start autocaching model instances, add a CacheController to your model:

.. code-block:: python

    from django.db import models
    from cachemagic import CacheController

    class myModel(models.Model):
        f1 = models.IntegerField()
        f2 = models.TextField()

        cache = CacheController()

    myModel.cache.get(pk=27)

When using cachemagic, you should avoid django operations that update multiple
rows at once, since these operations typically don't emit the signals that
cachemagic relies on for cache invalidation. This includes methods like `Queryset.update`_,
`Queryset.delete`_, and `RelatedManager.clear`_

Find the complete documentation at `django-cache-magic.readthedocs.org`_.


Thanks
------
Big thanks to `Travis Fischer`_ for drafting
a lot of documentation and tests!

.. _Queryset.update: https://docs.djangoproject.com/en/1.3/ref/models/querysets/#update
.. _Queryset.delete: https://docs.djangoproject.com/en/1.3/ref/models/querysets/#delete
.. _RelatedManager.clear: https://docs.djangoproject.com/en/1.3/ref/models/relations/#django.db.models.fields.related.RelatedManager.clear
.. _django-cache-magic.readthedocs.org: http://django-cache-magic.readthedocs.org/
.. _Travis Fischer: https://github.com/travisfischer
