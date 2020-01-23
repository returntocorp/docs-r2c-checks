# jinjalint-missing-doctype

**tl;dr** HTML documents must include a [`DOCTYPE` declaration](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)
at the beginning of the document. This prevents the browser from switching into
["quirks mode"](https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode).
Quirks mode can also impact the security of the web page.

## Description

Web pages missing a `DOCTYPE` declaration may be vulnerable to many different
esoteric forms of [XSS attacks](https://owasp.org/www-community/attacks/xss/),
such as Javascript execution via the [`<title>` tag or via a VML frame](https://html5sec.org/).

This check will detect the following case.

```html
<html>
    <body>
        ...
    </body>
</html>
```

The check will consider the following cases acceptable.

```html
<!DOCTYPE html>
<html>
    <body>
        ...
    </body>
</html>
```

```html
{% extends "header.html" %}
<div>
    ...
</div>
{% extends "footer.html" %}
```

This check only looks for a missing `DOCTYPE` in HTML documents that include
the `<html>` tag to avoid issues with templating inheritance.

## References

* [HTML `<!DOCTYPE>` Declaration](https://www.w3schools.com/tags/tag_doctype.asp)
* [Doctype](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)
* [Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* [XSS Vectors making use of HTML5 features](https://html5sec.org/)
* [Flask Template Inheritance](https://flask.palletsprojects.com/en/1.1.x/patterns/templateinheritance/)
