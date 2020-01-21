# r2c-jinjalint-anchor-href-template-variable

**tl;dr** The `href` attribute in anchor tags accepts the `javascript:` URI and is therefore susceptible to cross-site scripting (XSS) if a Jinja template variable is used to insert the link. This check will alert when a template variable is used in the `href` attribute of an anchor tag.

## Description

[Flask security recommendations](https://flask.palletsprojects.com/en/1.1.x/security/#cross-site-scripting-xss) warn about using Jinja variables to insert values into the `href` attribute of an anchor tag. The `javascript:` URI is valid in `href` attributes, which means the following XSS could happen:

```html
<a href="{{ value }}">click here</a>
<a href="javascript:alert('unsafe');">click here</a>
```

Use `url_for()`, a method in provided by [Flask](https://flask.palletsprojects.com/en/1.1.x/api/#flask.url_for) that is also [available in Jinja templates](https://flask.palletsprojects.com/en/1.1.x/tutorial/templates/), to generate links. 

You should also consider setting the [Content Security Policy (CSP)](https://flask.palletsprojects.com/en/1.1.x/security/#security-csp) header, which you can use to block inline JavaScript (such as the `javascript:` URI) from running. Set the `default-src` or `script-src` to `self`. We recommend using [Google's Flask Talisman](https://github.com/GoogleCloudPlatform/flask-talisman) for securing your Flask application.

For more information on the Content Security Policy, refer to the references.

This check will detect the following case.

```html
<html>
    <body>
        <a href="{{ value }}">Test</a>
    </body>
</html>
```

The check will consider the following case acceptable.

```html
<html>
    <body>
        <a href="{{ url_for('foo') }}">Test</a>
    </body>
</html>
```

## References

* [Unsafe href use with Jinja template variables](https://flask.palletsprojects.com/en/1.1.x/security/#cross-site-scripting-xss)
* [SettingCSP headers in Flask](https://flask.palletsprojects.com/en/1.1.x/security/#security-csp)
* [Content Security Policy](https://csp.withgoogle.com/docs/index.html)
* [MDN Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
* [MDN Content Security Policy Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)
* [Security Stack Exchange: Blocking Javascript in href](https://security.stackexchange.com/questions/203340/does-csp-block-javascript-in-a-href-tags)
* [Flask Talisman by Google](https://github.com/GoogleCloudPlatform/flask-talisman)