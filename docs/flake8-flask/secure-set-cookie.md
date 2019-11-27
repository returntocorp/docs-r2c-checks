# r2c-flask-secure-set-cookie

**tl;dr**: This check follows [Internet cookie best practices](https://techblog.topdesk.com/security/cookie-security/) to help you keep your Flask application’s cookies safe by ensuring they are only sent over HTTPS, unreadable by scripts, and by not blindly sending cookies to anyone who asks. This check alerts when the `secure`, `httponly`, and `samesite` keyword arguments are not supplied to `response.set_cookie`.

## Description

Modern web browsers support some great features for keeping web cookies secure. Lots of things can go wrong with cookies:  they can be sniffed in transit, scanned by a rogue script, or slurped up by mischevious domains.

Three of these settings [recommended by Flask](https://flask.palletsprojects.com/en/1.1.x/security/#set-cookie-options) are:

* `secure` - only allows cookies to be transmitted over HTTPS
* `httponly` - prevents scripts from reading cookies
* `samesite` - blocks cookies from being sent with cross-site requests

Under the hood, web servers send HTTP headers to the browser instructing it how to handle cookies. Flask supports setting these headers conveniently through the keyword arguments `secure`, `httponly`, and `samesite` on `response.set_cookie`.

This check encourages using these security features by alerting on calls to `response.set_cookie` without all three keyword arguments. However, the check does not enforce the settings to be "on." In doing so, the check forces a developer to be explicit about their security settings while allotting for situations where the settings need to be disabled.

This check will alert on these cases, for example:

``` python
from flask import make_response
@app.route("/none_set")
def none_set():
    response = make_response()
    response.set_cookie("cookie_name", "cookie_value")
    return response

@app.route("/some_set")
def some_set():
    r = make_response()
    
    # some values are set but not others
    r.set_cookie("cookie1", "cookie_value", secure=True)
    r.set_cookie("cookie2", "cookie_value", httponly=True)
    r.set_cookie("cookie3", "cookie_value", samesite='Lax')
    r.set_cookie("cookie4", "cookie_value", secure=True, httponly=True)
    r.set_cookie("cookie5", "cookie_value", httponly=True, samesite='Lax')
    return r
```

This check will _not_ alert on these cases:

``` python
import flask
@app.route("/")
def index():
    response = flask.make_response()
    response.set_cookie("cookie1", "cookie_value", secure=True, httponly=True, samesite='Lax')
    response.set_cookie("cookie2", "cookie_value", secure=True, httponly=True, samesite='Strict')
    response.set_cookie("cookie3", "cookie_value", secure=False, httponly=False, samesite=None)
    return response
```

## References

- https://techblog.topdesk.com/security/cookie-security/ - general cookie security guidelines
- https://www.owasp.org/index.php/SecureFlag
- https://www.owasp.org/index.php/HttpOnly
- https://www.owasp.org/index.php/SameSite
- https://github.com/GoogleCloudPlatform/flask-talisman - Google's Flask wrapper which enables `secure` and `httponly` for session cookies by default (but not for `set_cookie`)
- https://stackoverflow.com/questions/45218195/set-secure-attribute-for-flask-cookies
