
# avoid-hardcoded-config

From [builtin config variables](https://flask.palletsprojects.com/en/1.1.x/config/?highlight=configuration#builtin-configuration-values), this check discourages hardcoded usages of `ENV`, `DEBUG`, `TESTING`, and `SECRET_KEY` variables.


## Description

[Flask Configuration](https://flask.palletsprojects.com/en/1.1.x/config/?highlight=configuration#environment-and-debug-features) recommends using OS environment variables to setup up *Flask* config variables and strongly discourage hardcoding it because such hardcoded values can’t be read early by the flask command, and some systems or extensions may have already configured themselves based on a previous value.

The reason `ENV`, `DEBUG`, `TESTING` variables are selected among 29 builtin variables is that they enable environment and debug features that are used among almost all flask applications. `SECRET_KEY` is related to the sessions cookie and hardcoding it is a security vulnerability [CWE-798](https://cwe.mitre.org/data/definitions/798.html).

This check will alert on these cases, for example:

``` python
# SHOULD MATCH

# For `TESTING`
app.config["TESTING"] = True
app.config["TESTING"] = False
app.config.update(TESTING=True)

# For `SECRET_KEY`
app.config.update(SECRET_KEY="aaaa")
app.config["SECRET_KEY"] = b'_5#y2L"F4Q8z\n\xec]/'

# For `ENV`
app.config["ENV"] = "development"
app.config["ENV"] = "production"

# For `DEBUG`
app.config["DEBUG"] = True
app.config["DEBUG"] = False
app.run(debug=True)
```

This check will _not_ alert on these cases:

``` python
# SHOULD NOT MATCH
# For `TESTING`
app.config["TESTING"] = os.getenv("TESTING")
app.config["TESTING"] = "aa"

# For `SECRET_KEY`
app.config.update(SECRET_KEY=os.getenv("SECRET_KEY"))
app.config.update(SECRET_KEY=os.environ["SECRET_KEY"])

# FOR `ENV`
app.config["ENV"] = os.environ["development"]

# For `DEBUG`
app.config["DEBUG"] = os.environ["DEBUG"] or True
app.config["DEBUG"] = os.environ["DEBUG"] or False
app.run(debug=os.getenv("DEBUG", True))
```

## References
1. https://flask.palletsprojects.com/en/1.1.x/config/?highlight=configuration#builtin-configuration-values
2. https://flask.palletsprojects.com/en/1.1.x/config/?highlight=configuration#environment-and-debug-features