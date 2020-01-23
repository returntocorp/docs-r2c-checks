# jinjalint-unquoted-attribute-template-variable

**tl;dr** Quotes must be included around attribute values when using Jinja
template variables. Missing quotes can lead to XSS.

## Description

From [Flask security considerations](https://flask.palletsprojects.com/en/1.1.x/security/#cross-site-scripting-xss):

> Another thing that is very important are unquoted attributes. While Jinja2
> can protect you from XSS issues by escaping HTML, there is one thing it
> cannot protect you from: XSS by attribute injection. To counter this possible
> attack vector, be sure to always quote your attributes with either double or
> single quotes when using Jinja expressions in them.

This check will detect the following case.

```html
<html>
    <body>
        <input value={{ value }}>
    </body>
</html>
```

The check will consider the following cases acceptable.

```html
<html>
    <body>
        <input value="{{ value }}">
        <input value='{{ value }}'>
    </body>
</html>
```

## References

* [Flask Security Considerations](https://flask.palletsprojects.com/en/1.1.x/security/#cross-site-scripting-xss)
