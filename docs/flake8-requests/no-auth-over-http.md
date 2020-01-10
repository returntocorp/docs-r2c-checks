
# r2c-requests-no-auth-over-http

**tl;dr**: This check detects when the `auth` keyword argument is used over `http://`, which could expose credentials.

# Description

For [`requests`](https://requests.readthedocs.io/en/master/), the API-s do not guarentee that we use HTTP over SSL
even when we transport sensitive information like auth token. This introduces [CWE-522: Insufficiently Protected Credentials vulnerability](https://cwe.mitre.org/data/definitions/522.html).
It is a security hazard to use HTTP with authentication tokens. Use HTTPS as detailed in [rfc2818](https://tools.ietf.org/html/rfc2818).


This check will alert on these cases, for example:

``` python
import requests
r = requests.get('http://MYURL.com', auth=('user', 'pass'))


from requests import get, post
post('http://MYURL.com', auth=('user', 'pass'))
```

This check will catch above pattern. However, it will not fire on

```python
import requests
r = requests.get('http://MYURL.com')

# No import
r = requests.get('http://MYURL.com', auth=('user', 'pass'))
```

```
$> flake8 --select=r2c example.py
example.py:5:1: r2c-requests-no-auth-over-http auth is possibly used over http://, which could expose credentials. possible_urls: ['http://MYURL.com']
```

## References
- [requests documentation](https://requests.readthedocs.io/en/master/)
- [CWE-522: Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
- [RFC2818: HTTP Over TLS](https://tools.ietf.org/html/rfc2818)
