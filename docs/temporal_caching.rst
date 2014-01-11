================
Temporal Caching
================

Cached is a simple decorator that can be applied to any function to cache
results. By default it uses the arguments as a key, but both the key
and timeout can be customized.::

    from django.db import models
    from cachemagic.decorators import cached

    @cached
    def do_expensive_operation(thing, other):
        return [other(item) for item in MyModel.objects.where(a=thing)]


Using in model methods
======================
In most cases you should use an object's primary key as the cache key
instead of serializing the entire object.::

    from django.db import models
    from cachemagic.decorators import cached

    class Model(models.Model):
        field1 = IntegerField()
        field2 = TextField()

        @cached(key=lambda self: self.pk, timeout=180)
        def get_my_related_things(self):
            return [other(item) for item in self.related_things.select_related()]



