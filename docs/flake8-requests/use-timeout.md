
# r2c-requests-use-timeout

**tl;dr**: This check finds `requests` methods without a timeout. Without a timeout, `requests` will hang forever.

# Description

The [`requests` documentation](https://requests.readthedocs.io/en/master/user/quickstart/#timeouts) states that timeouts should be used in production code.

> You can tell Requests to stop waiting for a response after a given number of seconds with the timeout parameter. Nearly all production code should use this parameter in nearly all requests. Failure to do so can cause your program to hang indefinitely.

This check will detect the following case:

``` python
import requests
url = "https://github.com"
r = requests.get('{url}')
```

This case is considered acceptable:

```python
import requests
url = "https://github.com"
r = requests.get(url, timeout=10)
```

## References
- [`requests` documentation](https://requests.readthedocs.io/en/master/user/quickstart/#timeouts)