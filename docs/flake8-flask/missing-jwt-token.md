# r2c-flask-missing-jwt-token

**tl;dr**: JSON Web Tokens (JWT) tokens are used for authentication in web services. [Flask](https://palletsprojects.com/p/flask/) packages such as `flask_jwt`, `flask_jwt_extended`, and `flask_jwt_simple` provide a framework for assigning access tokens and verifying tokens for access to Flask routes. This check catches cases where the authentication decorators may be missing from certain routes and recommends their usage for API data security. 

## Description

This check alerts when `@jwt_required`, `@jwt_optional`, `@fresh_jwt_required`, and `@jwt_refresh_token_required` decorators are missing in files where `flask_jwt`, `flask_jwt_extended`, or `flask_jwt_simple` packages are imported. 

Since the packages allow users to implement custom token verification, the check does not count instances where a `JWTManager()` or `JWT()` function call is used to set up an authentication response handler.

``` python
## Missing decorators
import flask_jwt

@app.route('/protected')
def protected():
    return
```

The check considers these cases acceptable.

``` python
## @jwt_required decorator
import flask_jwt

@app.route('/protected')
@jwt_required
def protected():
    return


## Implementing JWT tokens
import flask_jwt

jwt = JWT(app, authenticate, identity)

@app.route('/protected')
def protected():
    return


## Implementing JWTManager
import flask_jwt

jwt = JWTManager()

@app.route('/protected')
def protected():
    return
```

## References

- https://assets.ctfassets.net/2ntc334xpx65/o5J4X472PQUI4ai6cAcqg/13a2611de03b2c8edbd09c3ca14ae86b/jwt-handbook-v0_14_1.pdf
