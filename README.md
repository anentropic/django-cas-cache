django-cas-cache
================

Django cache backend (currently memcache-only) supporting CAS (compare-and-set)
operations.

Provided as a mixin so you can add CAS support to your existing customised
backend. Or just use the backend provided: 

```python
CACHES = {
    'default': {
        'BACKEND': 'cascache.backends.memcached.MemcachedCache',
        'LOCATION': '127.0.0.1:11211',
    }
}
```

The idea of Compare-And-Set is that your new value is based on the existing
value of the key. So instead of a new value you should pass in an update
function to calculate the new value:

```python
cache = get_cache('default')
success = cache.cas('mykey', lambda x: x * 2)
```
