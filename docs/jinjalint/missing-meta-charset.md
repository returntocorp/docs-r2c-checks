# jinjalint-meta-charset

**tl;dr** HTML documents should include a [`<meta>` `charset` declaration](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)
in the `<head>` of the document. This prevents the browser from incorrectly
interpreting the character encoding of the document. The character encoding can
impact the security of the web page.

## Description

Web pages missing a `<meta>` `charset` declaration may be vulnerable to many different
esoteric forms of [XSS attacks](https://owasp.org/www-community/attacks/xss/),
such as Javascript execution via [`CESU-8`, `UTF-7`, `BOCU-1`, or `SCSU`
encoding](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta).

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
        <meta charset="UTF-8">
    </head>
    <body>
        ...
    </body>
</html>
```

## References

* [HTML meta charset Attribute](https://www.w3schools.com/TAGs/att_meta_charset.asp)
* [`<meta>`: The Document-level Metadata element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)
* [Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* [XSS Vectors making use of HTML5 features](https://html5sec.org/)
* [`UTF-7` XSS](https://code.google.com/archive/p/doctype-mirror/wikis/ArticleUtf7.wiki)
