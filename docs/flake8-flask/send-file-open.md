# r2c-flask-send-file-open

**tl;dr**: This check locates calls to `flask.send_file()` which will throw a runtime error. `flask.send_file()` throws a `ValueError` when `open([filename], [mode])` is passed as the first argument without the either the `mimetype` or `attachment_filename` keyword arguments.

## Description

Flask, as of v0.12, no longer infers the name attribute of file-like objects because the `name` attribute is not guaranteed. See [pallets/flask#104](https://github.com/pallets/flask/issues/104) for the discussion.

Now, sending a file-like object in `send_file` without specifying either `mimetype=` or `attachment_filename=` keyword arguments will raise a `ValueError` (albeit with a helpful message!).

This check alerts on the usage of a file-like object without satisfying the other conditions, preventing a runtime error.

For example, the following snippet,

``` python
import flask
@app.route("/send_file")
def send_file():
    f = open("test.db", 'r')
    rv = flask.send_file(f)
    rv.close()
```

Has the following stack trace.

``` python
[2019-10-30 10:25:14,695] ERROR in app: Exception on /send_file [GET]
  [cut for brevity...]
  File "example.py", line 5, in send_file
    rv = flask.send_file(f)
  File "/usr/local/lib/python3.7/site-packages/flask/helpers.py", line 593, in send_file
    "Unable to infer MIME-type because no filename is available. "
ValueError: Unable to infer MIME-type because no filename is available. Please set either `attachment_filename`, pass a filepath to `filename_or_fp` or set your own MIME-type via `mimetype`.
```

This check will catch this pattern.

```
$> flake8 --select=r2c-flask example.py
example.py:5:10: r2c-flask-send-file-open passing a file-like object to flask.send_file without a mimetype or attachment_filename will raise a ValueError
```

## References

- [Flask Github issue discussion: send_file in Flask 0.6](https://github.com/pallets/flask/issues/104)
- [Flask PR: properly remove f.name usage in send_file](https://github.com/pallets/flask/pull/1988)
- [Instructions on upgrading Flask](https://github.com/pallets/flask/blob/ad567480708ceb860fab0455d3220b92ecd67601/docs/upgrading.rst)
