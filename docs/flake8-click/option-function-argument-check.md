# r2c-click-option-function-argument-check

**tl;dr**: This check makes sure that parameters match in `@click.option` and the function definition.

The check will fire in the following cases:

``` python
@click.command()
@click.option('-d', '--dummy')
def build(foo): pass

@click.command()
@click.option('-d', '--dummy')
@click.option('-f', '--fummy')
def build(foo): pass
```

This check considers the following cases acceptable:

``` python
@click.command()
@click.option()
def run(y): pass

@click.command()
@click.option(x)
def run(y): pass

@click.command()
@click.option('-d', '--dummy')
def build(dummy): pass
```

## References

* [More on Click options](https://click.palletsprojects.com/en/7.x/options/)
