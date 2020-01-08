# r2c-flask-use-jsonify

**tl;dr**: `flask.jsonify()` is a [Flask](https://palletsprojects.com/p/flask/) helper method which handles the correct settings for returning JSON from Flask routes. This check catches uses of `json.dumps()` returned from Flask routes and encourages `flask.jsonify()` instead.

## Description

`flask.jsonify()` is a [Flask](https://palletsprojects.com/p/flask/) helper method which handles the correct settings for returning JSON from Flask routes. This check catches uses of `json.dumps()` returned from Flask routes and encourages `flask.jsonify()` instead.

The check will detect these cases.

``` python
## Normal import
import flask
import json
app = flask.Flask(__name__)

@app.route("/user")
def user():
    user_dict = get_user(request.args.get("id"))
    return json.dumps(user_dict)


## Method import
import flask
from json import dumps
app = flask.Flask(__name__)

@app.route("/user")
def user():
    user_dict = get_user(request.args.get("id"))
    return dumps(user_dict)
```

The check considers these cases acceptable.

``` python
## flask.jsonify
import flask
app = flask.Flask(__name__)

@app.route("/user")
def user():
    user_dict = get_user(request.args.get("id"))
    return flask.jsonify(user_dict)


## Import jsonify directly
from flask import Flask, jsonify
app = Flask(__name__)

@app.route("/user")
def user():
    user_dict = get_user(request.args.get("id"))
    return jsonify(user_dict)


## Not json.dumps
import json
from flask import Flask
from bson import dumps
app = Flask(__name__)

@app.route("/user")
def user():
    user_dict = get_user(request.args.get("id"))
    return dumps(user_dict)
```

## References

- [Stack Overflow: `json.dumps()` vs. `jsonify()`](https://stackoverflow.com/questions/7907596/json-dumps-vs-flask-jsonify)