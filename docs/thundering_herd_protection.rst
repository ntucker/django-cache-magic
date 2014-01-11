==========================
Thundering Herd Protection
==========================

When your cache keys expire, there is a time window before the new value is recomputed where many clients will
not be able to retrieve any result. This will cause a huge load to database backends. More can be found here: 
http://en.wikipedia.org/wiki/Thundering_herd_problem

To eliminate the problem, CacheMagic provides a redis backend that stores extra metadata about expiry time.
This allows one client to realize the key will expire soon and tell the others continue using the old value until
it is recomputed.

Usage
==================
To setup the protection, simply use the RedisHerdCache backend provided.
Here is an example configuration::

    CACHES['default'] = {
        'BACKEND': 'cachemagic.cache.RedisHerdCache',
        'LOCATION': ':'.join([REDIS_HOST, str(REDIS_PORT), '0']),
        'OPTIONS': {
            'PASSWORD': REDIS_PASSWORD,
        },
        'VERSION': 0,
    }
