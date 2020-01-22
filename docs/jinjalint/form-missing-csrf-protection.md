# r2c-jinjalint-form-missing-csrf-protection

**tl;dr** Flask apps using Flask-WTF require including a CSRF token in the HTML template itself. This check detects missing CSRF protection in HTML forms in Jinja templates.

## Description

The [Flask-WTF documentation](https://flask-wtf.readthedocs.io/en/stable/csrf.html) states that forms must render the CSRF token in the template. It is highly recommended that all forms are protected with CSRF tokens.

```html
<form method="post">
    {{ form.csrf_token }}
</form>
```

```html
<form method="post">
    <input type="hidden" name="csrf_token" value="{{ csrf_token() }}"/>
</form>
```

This is easy to forget. `jinjalint-form-missing-csrf-protection` will detect forms that are missing either of the above CSRF tokens.

```html
<html>
    <body>
        <form method="post">
            <input name="foo" value="bar"/>
        </form>
    </body>
</html>
```

The check will consider the following case acceptable.

```html
<html>
    <body>
        <form method="post">
            <input type="hidden" name="csrf_token" value="{{ csrf_token() }}"/>
        </form>
    </body>
</html>

<html>
    <body>
        <form method="post">
            {{ form.csrf_token }}
        </form>
    </body>
</html>
```

## References

* [What is CSRF?](https://portswigger.net/web-security/csrf)
* [CSRF in Flask-WTF](https://flask-wtf.readthedocs.io/en/stable/csrf.html)
* [Fix missing CSRF token issues with Flask](https://nickjanetakis.com/blog/fix-missing-csrf-token-issues-with-flask)