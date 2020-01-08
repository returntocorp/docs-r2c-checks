# r2c-click-launch-uses-literal

**tl;dr**: This check looks for non-literal URLs in `click.launch()`, which could direct a browser to a malicious site.


## Description
`click.launch()` will open a browser to the given website. This can be dangerous if the site is not known and trusted. This check will detect where non-literal urls are used as an argument to `click.launch()`.

The check will fire in the following cases:

``` python
## Variable in click.launch
import click
click.launch(x)

from click import launch
launch(x)

## Import aliasing
import click as c
c.launch(x)

from click import launch as cli_launch
cli_launch(x)

## As a keyword argument
import click
click.launch(url=x)
```

This check considers the following cases acceptable:

``` python
## Literal string in launch
import click
click.launch("https://google.com")

## As a keyword argument
from click import launch
launch(url="https://bing.com")
```

## References

* [Click documentation on launching external applications](https://click.palletsprojects.com/en/7.x/utils/#launching-applications)
