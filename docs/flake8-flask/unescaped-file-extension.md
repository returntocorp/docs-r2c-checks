# r2c-flask-unescaped-file-extension

**tl;dr**  Flask will not autoescape Jinja templates that do not have `.html`, `.htm`, `.xml`, or `.xhtml` as extensions. This check will alert you if you do not have one of these extensions. This check will also do its best to detect if context variables are escaped if a non-escaped extension is used.

## Description

Flask does not autoescape Jinja templates that do not have the `.html`, `.htm`, `.xml`, or `.xhtml` file extensions. This behavior is described in the Flask documentation here: <https://flask.palletsprojects.com/en/1.1.x/templating/#jinja-setup>.

--------
Flask will autoescape in this case:

``` python
@app.route("/safe")
def safe():
    return render_template("safe.html", hello=request.args.get("hello"))
```


<div style="border: 1px solid grey">
    <img src="../../images/unescaped-template-file-extension-safe.png">
</div>

--------
But, Flask will not autoescape in this case:

``` python
@app.route("/unsafe")
def unsafe():
    return render_template("unsafe.txt", hello=request.args.get("hello"))
```

<div style="border: 1px solid grey">
    <img src="../../images/unescaped-template-file-extension-unsafe.png">
</div>

--------

## References

* https://flask.palletsprojects.com/en/1.1.x/templating/#jinja-setup
* https://flask.palletsprojects.com/en/1.1.x/api/#template-rendering
* https://docs.openstack.org/bandit/latest/plugins/b701_jinja2_autoescape_false.html - bandit check for not autoescaping jinja templates
* https://stackoverflow.com/questions/14592554/disabling-autoescape-in-flask
* https://nvisium.com/blog/2015/12/07/injecting-flask.html - article about SSTI and XSS in Flask
