# r2c-requests-use-scheme

**tl;dr**: This check finds URLs passed to  `requests` API methods don't have a URL scheme (e.g., https://), otherwise a `MissingSchema` exception will be thrown at runtime.


# Description

For [`requests`](https://requests.readthedocs.io/en/master/), the API-s consider the following to be valid URL scheme per [urllib](https://docs.python.org/3/library/urllib.parse.html)
```json
- file
- ftp
- gopher
- hdl
- http
- https
- imap
- mailto
- mms
- news
- nntp
- prosper,
- rsync
- rtsp
- rtspu
- sftp
- shttp
- sip
- sips
- snews
- svn
- svn+ssh
- telnet
- wais
- ws
```
This check will alert on these cases, for example:

``` python
r = requests.request("GET", "github.com"
r = requests.request("POST", "notavalidscheme://www.github.com/user?name=foo&repo=bar")
```

This check will catch above pattern. However, it will not fire on

```python
import requests
requests.get("http://github.com")
requests.get("http://localhost:8000")
requests.get("http://www.github.com/user?name=foo&repo=bar")
requests.get("https://github.com")
requests.get("https://www.github.com/user?name=foo&repo=bar")
requests.get("ftp://192.168.1.101")
requests.get("ws://192.168.1.101")
requests.get("file:///Users/user/secrets.txt)
```

```
$> flake8 --select=r2c example.py
example.py:8:5: r2c-requests-use-scheme need a scheme (e.g., https://) for one of these possible urls ['notavalidscheme://www.github.com/user?name=foo&repo=bar'] otherwise requests will throw an exception.  See https://stackoverflow.com/questions/15115328/python-requests-no-connection-adapters
```

## References
- [Stack Overflow about this issue](https://stackoverflow.com/questions/15115328/python-requests-no-connection-adapters)