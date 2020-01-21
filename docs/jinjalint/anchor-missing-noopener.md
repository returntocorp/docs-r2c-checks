# r2c-jinjalint-anchor-missing-noopener

**tl;dr** Pages opened with `target="_blank"` allow the new page to access the original's `window.opener`. This can have security, privacy, and performance implications. Include `rel="noopner noreferrer"` to prevent this.

## Description

[This page](https://developers.google.com/web/tools/lighthouse/audits/noopener) by Google explains the danger of `target="_blank"`. The short version is that a page opened with `target="_blank"` can access the `window` object of the origin page. It can also manipulate the `window.opener` property, which could redirect the origin page to a malicious URL. This is called [**reverse tabnapping**](https://owasp.org/www-community/attacks/Reverse_Tabnabbing).

In general, when using `target="_blank"`, always include `rel="noopener noreferrer`.

This check will detect the following cases.

```html
<!-- Missing rel= -->
<html>
    <body>
        <a href="https://example.com" target="_blank">Test</a>
    </body>
</html>

<!-- Missing "noopener" -->
<html>
    <body>
        <a href="https://example.com" target="_blank" rel="noreferrer">Test</a>
    </body>
</html>
```

The check will consider the following case acceptable.

```html
<!-- No target="_blank" -->
<html>
    <body>
        <a href="https://example.com">Test</a>
    </body>
</html>

<!-- rel="noopener noreferrer" exists -->
<html>
    <body>
        <a href="https://example.com" target="_blank" rel="noopener noreferrer">Test</a>
    </body>
</html>
```

## References

* [Links to cross-origin destinations are unsafe](https://developers.google.com/web/tools/lighthouse/audits/noopener)
* [GitHub discussion about rel="noopener noreferrer" for the Asciidoctor project](https://github.com/asciidoctor/asciidoctor/issues/2071)
* [Reverse Tabnapping](https://owasp.org/www-community/attacks/Reverse_Tabnabbing)
* [StackOverflow post explaining the `target="_blank"` vulnerability](https://stackoverflow.com/questions/50709625/link-with-target-blank-and-rel-noopener-noreferrer-still-vulnerable)