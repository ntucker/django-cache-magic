=====================================
Introduction
=====================================

CacheMagic addresses the most common scenarios for caching and cache
invalidation: `temporal caching`, `instance caching`, `related objects
caching`, and `thundering herd` protection.

Temporal Caching
================
This is like memoizing an expensive operation. Usually something that doesn't need to be super-fresh,
and aggregates many objects - making invalidation difficult or impossible.::

    @cached
    def my_expensive_call(arg1, arg2):
        return do_other_things(arg1) * arg2


Instance Caching
================
This is the practice of caching individual model instances. CacheMagic provides
a `CacheController` that you can attach to models to cause automatic caching and
invalidations. ::

    class Model(django.models.Model):
        cache = cachemagic.CacheController()
        field = django.models.TextField()

    Model.objects.get(pk=27)    # hits the database
    Model.cache.get(27)         # Tries cache first


Related Objects Caching
=======================
Having fetched an instance of a model, a frequent database operation is to
find all the instances of another model that are related to your instance via
foreign keys. You can attach a `RelatedCacheController` to your model to enable
automatic caching and invalidation of these relations. ::

    instance = Model.cache.get(pk=27)
    related_things = instance.things_set.all()  # hits the database
    related_things = instance.cache.things_set  # Tries cache first

The RelatedCacheController will automatically detect and cache objects related
to the model it resides on by ForeignKeys, ManyToManyFields, and
OneToOneFields.


Thundering Herd Protection
==========================
Any caching will be useless when a cache key expires and thousands of requests try to recompute the value
at the same time. CacheMagic provides a cache backend for redis that prevents this problem by designating
only one client to recompute the value while others simply read the existing cache value. ::

    CACHES['default'] = {
        'BACKEND': 'cachemagic.cache.RedisHerdCache',
        'LOCATION': ':'.join([REDIS_HOST, str(REDIS_PORT), '0']),
        'OPTIONS': {
            'PASSWORD': REDIS_PASSWORD,
        },
    }

