---
title: Caching with Apache mod_cache
---

# Caching with Apache mod_cache

To satisfy my own curiosity I did some more testing with mod_cache. Here’s my current configuration:

```
<IfModule mod_cache.c>
  CacheLock on
  CacheHeader on
  CacheDetailHeader on
  CacheIgnoreCacheControl on
  CacheIgnoreHeaders Set-Cookie
  CacheStoreExpired on
  <IfModule mod_cache_disk.c>
    CacheRoot /var/cache/httpd
    CacheEnable disk "/"
    CacheDirLevels 1
    CacheDirLength 1
  <IfModule>
<IfModule>

ProxyPreserveHost on
ProxyPass / http://example.com/
ProxyPassReverse / http://example.com/
```

Using apache bench (ab) I ran 100,000 requests on a WordPress site with 100 concurrent connections. The average 
response time was 35 ms, and 99% of requests were served within 60 ms.

The key cache directives are “CacheLock on” and “CacheStoreExpired on”.

“CacheStoreExpired on” tells the proxy to cache expired resources. When a client requests the resource, the proxy will 
send a conditional request to the server to revalidate the cached copy. If no newer version is available, it will serve 
the stale resource from the cache.

“CacheLock on” prevents the proxy from inundating the server with revalidation requests. Without the cache lock, the 
load test doesn’t work. 100 concurrent connections quickly swamp the backend server with revalidation requests, the 
number of httpd processes spikes, and the load average starts climbing. But with “CacheLock on”, the proxy only 
sends one validation request to the backend server at a time.

I also found “CacheIgnoreCacheControl on” invaluable for testing. When you refresh the browser, it sends 
a “Cache-Control: no-cache” header to the proxy, so you are never getting a cached response. This directive 
tells the proxy to ignore the browser’s cache control header, so you can actually test the cached responses.
