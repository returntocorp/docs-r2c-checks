# jinjalint-meta-charset

**tl;dr** HTML documents should include a [`<meta>` `Content-Type` declaration](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)
in the `<head>` of the document. This declaration provides defense-in-depth
against the browser incorrectly interpreting the character encoding of the
document. The character encoding can impact the security of the web page.

## Description

Web pages missing a `<meta>` `Content-Type` declaration may be vulnerable to many different
esoteric forms of [XSS attacks](https://owasp.org/www-community/attacks/xss/),
such as Javascript execution via [`CESU-8`, `UTF-7`, `BOCU-1`, or `SCSU`
encoding](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta).

This declaration provides additional defense-in-depth on top of setting the
`<meta>` `charset`. Including the `<meta>` `Content-Type` provides protection
when using the `file://` protocol or when using older browsers.

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
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    </head>
    <body>
        ...
    </body>
</html>
```

## References

* [HTML `<meta>` `http-equiv` Attribute](https://www.w3schools.com/tags/att_meta_http_equiv.asp)
* [`<meta>`: The Document-level Metadata element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)
* [Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* [XSS Vectors making use of HTML5 features](https://html5sec.org/)
* [`UTF-7` XSS](https://code.google.com/archive/p/doctype-mirror/wikis/ArticleUtf7.wiki)
