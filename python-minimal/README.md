# Cookiecutter template for Python

This template is designed for simple data science project in Python.
It suggests a combination of notebooks and a module for shared utilities.
Development tools are kept to a minimum.

Python 3.12 is chosen as a minimum, as 3.11 will reach end-of-life in 2027 (cf. [Python Release Cycle](https://devguide.python.org/versions/#python-release-cycle)).


## Dependencies and reproducibility

The Python ecosystem has evolved over time, and standards are trying to catch up with common practices and major tools.
As of 2026, [`pyproject.toml`](https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/) is well-supported and acts as the main definition point of both project metadata (cf. [PEP 621](https://peps.python.org/pep-0621/)) and tools configurations.
Please refer to the [PyPA specification documentation](https://packaging.python.org/en/latest/specifications/section-distribution-metadata/) for full details.

Dependencies used in a project are divided into these standardized fields:

 * `project.dependencies`: these represent the core runtime dependencies, that should be included in any installation of the project. They get installed with `pip install my-project` by default.
 * `project.optional-dependencies`: these extra dependencies are optional, often for opt-in installable features, installed using `pip install my-project[feature]`.
 * `dependency-groups` ([PEP 735](https://peps.python.org/pep-0735/)): these development dependencies are not included as part of the distributed package, and need to be explicitly using `pip install --group dev`.

Dependencies versions (cf. [semantic versioning](https://packaging.python.org/en/latest/discussions/versioning/)) should usually be as broad as possible, to reduce the incompatibility with other libraries and tools.
While there is no automated way to infer the largest range that still works for a project, a few rules can be applied:

 1. Only specify direct dependencies, often the ones that are imported explicitly in the code. Exceptions include, for instance, `openpyxl` to read Excel files using Pandas, which is usually not explicitly imported.
 2. Always constrain the major version, as libraries are usually introducing breaking changes. For unstable libraries (i.e., before version 1.0), treat minor version changes as breaking as well.
 3. Start simple, using the tilde operator, such as `"pandas~=2.1"` and `"ruff~=0.12.3"`, which are equivalent to `"pandas>=2.1; pandas<3"` and `"ruff>=0.12.3; ruff<0.13"`, respectively.
 4. Try to lower the constraint later on. For instance, if a project only uses a few functions from `scipy`, check the documentation to see when they were introduced.

For reproducibility, keep a pinned (locked) record of *exact* versions used during development.
Historically, this relied on compiling `requirements.in` (now replaced by the aforementioned fields in `pyproject.toml`) into `requirements.txt`.
In practice, the usage may differ; writing manually one or multiple `requirements.txt` files is also common.

It is important to note that dependencies may differ according to the Python version and the operating system (cf. [PyPA](https://packaging.python.org/en/latest/specifications/dependency-specifiers/)).
While this is often not needed for project dependencies, the exhaustive list of pinned dependencies is often not portable without explicit environment markers.

Even if these markers are supported by `requirements.txt`, modern tools use instead more structured "lock" files, which includes more information about each dependency.
For instance, this allows the use of multiple indices, a strategy used by [PyTorch](https://pytorch.org/) to support different hardwares.

[PEP 751](https://peps.python.org/pep-0751/) recently introduced `pylock.toml` (cf. [PyPA](https://packaging.python.org/en/latest/specifications/pylock-toml/)) as a standard lock file.
Unfortunately, tool support is still incomplete; in practice, a `pylock.toml` is more akin to an exchange format between tool-specific lock files.
Notably, `pip` cannot install from a `pylock.toml` file [yet](https://github.com/jazzband/pip-tools/issues/2124).

Among third-party tools, [`uv`](https://docs.astral.sh/uv/) is a popular solution to many Python environment- and dependency-related aspects.
In particular, it supports the use and generation of lock files (both `uv.lock` and `pylock.toml`).
See the [documentation](https://docs.astral.sh/uv/guides/migration/pip-to-project/) for more details on why and how `uv` replaces `pip`.


## Development tools

Two main reasons are often discussed when promoting the use of tools during development:

 * **Consistency**: ensuring the same workflow and conventions across a project or team ease collaboration, by promoting a standardized style and avoiding surprises.
 * **Automation**: many simple fixes and verifications can be automatically applied, avoiding manual operations.

In both cases, the core objective is to reduce the mental workload on the developper.
This is why using a combination of properly configured tools is crucial for productivity and quality:

 1. The IDE (e.g. VSCode, PyCharm) is the typical user interface when writing code. Enabling tools during development is key.
 2. Then, [pre-commit](https://pre-commit.com/) hooks ensure that a set of rules are applied before committing changes. To avoid blocking the development pace, these should be quick operations.
 3. Finally, continuous integration (CI) is running server-side, sending notifications should a check fail. As this run asynchronously, long-running tests are fine.

A notable example of development tool is the code formatter.
While many alternatives exist, [Ruff](https://docs.astral.sh/ruff/) (following the [Black](https://black.readthedocs.io/en/stable/the_black_code_style/index.html) code style) is a common choice.
To ensure that projects all follow the same style, and avoid opinionated debates, it is *not* configurable (apart from the overall line length).

Another family of tools include static analysis, inspecting the codebase without running it.
Ruff also cover this point to some extent, replacing efficiently [Pylint](https://pylint.pycqa.org/en/latest/index.html), [Flake8](https://flake8.pycqa.org/en/latest/index.html), and [isort](https://pycqa.github.io/isort/).

Static analysis also benefits from type hints, as type checkers (e.g. [mypy](https://mypy-lang.org/), [pyright](https://microsoft.github.io/pyright/)) will check their validity and help spot issues in the code.


## Packaging tools

During development, it is common practice to install the current module in [editable mode](https://setuptools.pypa.io/en/latest/userguide/development_mode.html).
The [`src` layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/) is often used to enforce this practice.
The main advantage is to avoid reliance on the current working directory (e.g. through manual updates using `sys.path.append`; a fragile approach) by installing it in the virtual environment.

As discussed above, `pyproject.toml` is the modern approach to project configuration, which includes the packaging of a module.
The exact definition depends on the [build system](https://pip.pypa.io/en/stable/reference/build-system/) used.
[PyPA](https://www.pypa.io/en/latest/)'s [Setuptools](https://setuptools.pypa.io/en/latest/) is proposed as build backend, as this is historically the most common solution.
However, other options are discussed in [Python Packaging User Guide](https://packaging.python.org/en/latest/), such as [Hatch](https://hatch.pypa.io/latest/) or [`uv`](https://docs.astral.sh/uv/concepts/build-backend/).

For most cases, this is a replacement for `setup.py`.
However, `setup.py` is not being [deprecated](https://packaging.python.org/en/latest/discussions/setup-py-deprecated/) as [C extensions](https://setuptools.pypa.io/en/latest/userguide/ext_modules.html) still require this file.

Note that no default license file is provided, as this template does not necessarily targets open source projects.
By default, copyright applies; [choose a license](https://choosealicense.com/) if you would like to open your project!
