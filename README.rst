## Quick Start
### Install sphinx
```shell
$ python -m venv .venv
$ source .venv/bin/activate
(.venv) $ python -m pip install sphinx
```


### Install theme
```shell
$ pip install sphinx_rtd_theme
```


## Build local preview
```shell
$ sphinx-build -M html docs/source/ build/
```
Then, open index.html in the build/html directory with a browser to preview.