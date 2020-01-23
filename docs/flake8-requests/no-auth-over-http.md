
# r2c-requests-no-auth-over-http

**tl;dr**: This check detects when the `auth` keyword argument is used over `http://`, which exposes credentials.

# Description

For [`requests`](https://requests.readthedocs.io/en/master/), the APIs make no guarantee that HTTPS is used 
when we transport sensitive information. Credentials should be protected using HTTPS ([CWE-522: Insufficiently Protected Credentials vulnerability](https://cwe.mitre.org/data/definitions/522.html)), as it is a security hazard to use HTTP with authentication tokens.

This check will alert on these cases, for example:

``` python
import requests
r = requests.get('http://example.com', auth=('user', 'pass'))


from requests import get, post
post('http://example.com', auth=('user', 'pass'))
```

However, it will not fire on:

```python
import requests
r = requests.get('http://example.com')

# Uses https
import requests
r = requests.post('https://example.com', auth=('user', 'pass'))

# No import
r = requests.get('http://example.com', auth=('user', 'pass'))
```

```
$> flake8 --select=r2c example.py
example.py:5:1: r2c-requests-no-auth-over-http auth is possibly used over http://, which could expose credentials. possible_urls: ['http://example.com']
```

## References
- [requests documentation](https://requests.readthedocs.io/en/master/)
- [CWE-522: Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
- [RFC2818: HTTP Over TLS](https://tools.ietf.org/html/rfc2818)
