
# r2c-flask-use-blueprint-for-modularity

**tl;dr**: This check recommends using Blueprint when there are too many route handlers in a single file. Blueprint encourages modularity and [can greatly simplify how large applications work and provide a central means for Flask extensions to register operations on applications.](https://flask.palletsprojects.com/en/1.1.x/blueprints/#blueprints)


## Description
The check fires when there are more than 3 routes with the same prefix in a file.
Definitions that are very long and the user does not seem to be using blueprints!

The check will detect these cases.
``` python
@app.route("/user")
def user():
    return
@app.route("/user/a")
def user_b():
    return
@app.route("/user/b")
def user_b():
    return
```

## References
- https://flask.palletsprojects.com/en/1.1.x/blueprints/#blueprints