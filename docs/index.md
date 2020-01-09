# Bento Checks

Documentation for [r2c](https://r2c.dev)'s custom checks. These checks are integrated into our program analysis tool, [Bento](https://bento.dev).

You can get these along with other great open source checks by installing Bento!

```
$ pip3 install bento-cli
```

See [Bento's README](https://bento.dev) for more details.

## List of Checks

### __flake8-flask__

[**r2c-flask-send-file-open**](flake8-flask/send-file-open/)

This check locates calls to `flask.send_file()` which will throw a runtime error. `flask.send_file()` throws a `ValueError` when `open([filename], [mode])` is passed as the first argument without the either the `mimetype` or `attachment_filename` keyword arguments.

[**r2c-flask-secure-set-cookie**](flake8-flask/secure-set-cookie/)

This check detects calls to `response.set_cookie` that do not have `secure`, `httponly`, and `samesite` set. This follows the guidance in the [Flask documentation](https://flask.palletsprojects.com/en/1.1.x/security/#set-cookie-options).

[**r2c-flask-unescaped-file-extension**](flake8-flask/unescaped-file-extension/)

Flask will not autoescape Jinja templates that do not use `.html`, `.htm`, `.xml`, or `.xhtml` as extensions. This check will alert you if you do not have one of these extensions.

[**r2c-flask-use-jsonify**](flake8-flask/use-jsonify/)

`flask.jsonify()` is a [Flask](https://palletsprojects.com/p/flask/) helper method which handles the correct settings for returning JSON from Flask routes. This check catches uses of `json.dumps()` returned from Flask routes and encourages `flask.jsonify()` instead.

[**r2c-flask-use-blueprint-for-modularity**](flake8-flask/use-blueprint-for-modularity)


This check recommends using Blueprint when there are too many route handlers in a single file. Using Blueprint encourages modularity and [can greatly simplify how large applications work and provide a central means for Flask extensions to register operations on applications.](https://flask.palletsprojects.com/en/1.1.x/blueprints/#blueprints)

### __sgrep-flask__

[**avoid-hardcoded-config**](sgrep-flask/avoid-hardcoded-config/)

This check discourages hardcoded usages of ENV, DEBUG, TESTING, and SECRET_KEY variables in Flask.

### __flake8-requests__

[**r2c-requests-no-auth-over-http**](flake8-requests/no-auth-over-http/)

This check detects when the `auth` keyword argument is used over `http://`, which could expose credentials.

[**r2c-requests-use-scheme**](flake8-requests/use-scheme/)

This check finds URLs passed to  `requests` API methods don't have a URL scheme (e.g., https://), otherwise a `MissingSchema` exception will be thrown at runtime.

### __flake8-boto3__

[**r2c-boto3-hardcoded-access-token**](flake8-boto3/hardcoded-access-token/)

This check looks for hardcoded AWS access tokens used in [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) API calls.
