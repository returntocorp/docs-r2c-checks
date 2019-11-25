# bento-checks

Documentation for [r2c](https://r2c.dev)'s custom checks. These checks are integrated into our program analysis tool, [Bento](https://bento.dev).

You can get these along with other great open source checks by installing Bento!

```
$ pip3 install bento-cli
```

## Usage

To get started right away with sensible defaults:

```
$ bento init && bento check
```

To set aside preexisting results so you only see issues in new code:

```
$ bento archive
```

Bento is at its best when run automatically as a Git pre-commit hook (i.e. bento install-hook) or as part of CI.

Contact us at support@r2c.dev. We want to hear from you!

## List of Checks

--------
[**r2c-flask-send-file-open**](flake8-flask/r2c-flask-send-file-open/)

This check detects the use of a file-like object in `flask.send_file` without one of either the `mimetype` or `attachment_filename` keyword arguments. `send_file` will throw a ValueError in this situation.

--------

[**r2c-flask-secure-set-cookie**](flake8-flask/r2c-flask-secure-set-cookie/)

This check detects calls to `response.set_cookie` that do not have `secure`, `httponly`, and `samesite` set. This follows the guidance in the [Flask documentation](https://flask.palletsprojects.com/en/1.1.x/security/#set-cookie-options).

--------

[**avoid-hardcoded-config**](sgrep-flask/avoid_hardcoded_config/)

This check discourages hardcoded usages of ENV, DEBUG, TESTING, and SECRET_KEY variables in Flask.

--------

[**r2c-flask-unescaped-template-file-extension**](flake8-flask/r2c-flask-unescaped-template-file-extension/)

This check detects the use of template file extensions that Flask does not escape by default. Flask only autoescapes templates with .html, .htm, .xml, and .xhtml extensions.
