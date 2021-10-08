# Contributing

## Quick install

```
pip install -e .[dev]
```

will install `h5grove` in editable mode with all the linting/formating/testing packages.

To install also [flask](https://flask.palletsprojects.com/en/) and [tornado](https://www.tornadoweb.org/en/stable/) packages, run

```
pip install -e .[all]
```

## Linting

- Formatted with [black](https://github.com/psf/black)
- Linted with [flake8](https://github.com/PyCQA/flake8)
- Type-checked with [mypy](https://github.com/python/mypy)

All lint checks can be run with

```
python setup.py lint
```

Configuration entries are located in `setup.cfg`.

**Code is checked at each push on main and for each PR** (see `lint-test.yml` workflow).

## Tests

Tests are handled with [pytest](https://docs.pytest.org/en/stable/index.html). These are mostly integration tests for now that tests the responses of the endpoints of the tornado and flask example apps.

Once `h5grove` is installed in editable mode, simply run the tests with: `pytest`.

### Benchmarks

Benchmarks of different formats (`json`, `.npy`) and web server frameworks (`flask`, `tornado`) are run as part of the tests. Those benchmarks are based on [pytest-benchmark](https://pytest-benchmark.readthedocs.io/en/latest/).

The test suite can be run without these benchmarks by running `pytest --benchmark-skip`.

## Release

Versioning is handled by [bump2version](https://github.com/c4urself/bump2version) (config in `.bumpversion.cfg`). To release a new version:

- Checkout `main` and ensure that it is up to date
- Be sure that lint checks, tests and documentation generation are passing
- Run `bump2version [patch|minor|major]`

This will create a commit increasing the version number and tag it in `git`. To trigger the PyPI release, push the commit and the tag to GitHub:

```
git push && git push --tags
```

The new tag will trigger the CI (`release.yml`) that will build and release the package on PyPI. If this succeeds, the job will also update the documentation at https://silx-kit.github.io/h5grove/ (more on this below).

### Documentation

The documentation is generated using [Sphinx](https://www.sphinx-doc.org/en/master/index.html) by parsing Markdown files with [myst-parser](https://myst-parser.readthedocs.io/en/latest/index.html).

The relevant files are in `docs/`:

- `conf.py`: the Sphinx configuration file
- `index.md`: Landing documentation page. It includes the `README.md`.
- `reference.md`: Generates API documentation for `content` and `encoders` modules using [sphinx.ext.autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html).

The documentation can be build locally using

```
python setup.py doc
```

The HTML pages will be generated in `_build`.