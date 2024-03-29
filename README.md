django-cas-cache
================

### NOTE: obviously this package is now superseded by the `get_or_set` method of cache backend in newer versions of Django https://docs.djangoproject.com/en/3.2/topics/cache/#django.core.caches.cache.get_or_set

Django cache backends supporting CAS (compare-and-set) operation. Memcache,
Redis and Dummy backends.

Provided as a mixin so you can add `cas` method to your existing customised
backend. Or use one of the ready-mixed backends provided:

```python
CACHES = {
    'default': {
        'BACKEND': 'cascache.backends.memcached.MemcachedCASCache',
        'LOCATION': '127.0.0.1:11211',
    }
}
```

The idea of Compare-And-Set is that your new value is based on the existing
value of the key, but it's a way to beat the race condition encountered with
naïve implementations. For more details see this blog post by Python's BDFL:
http://neopythonic.blogspot.co.uk/2011/08/compare-and-set-in-memcache.html

The pattern we use here is that instead of a new value you should pass in an
update function to calculate the new value:

```python
cache = get_cache('default')
success = cache.cas('mykey', lambda x: x * 2)
```

Redis backend adds a `cas_many` method analogous to `set_many`, takes a dict:

```python
cache = get_cache('default')
success = cache.cas_many({'key1': lambda x: x * 2, 'key2': lambda x: x / 2})
```
