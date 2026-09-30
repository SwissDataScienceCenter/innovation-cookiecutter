# {{ cookiecutter.project_name }}

This project is made using [this template](https://github.com/SwissDataScienceCenter/innovation-cookiecutter).
Next steps include:

 - [x] Create project from the Cookiecutter template.
 - [ ] Create a virtual environment to work in an isolated Python installation.
 - [ ] Install [pre-commit](https://pre-commit.com/) hooks, and run them once to generate the lock files, before the first push.
 - [ ] Update `authors` and `description`, in `pyproject.toml`.
 - [ ] Add development and installation dependencies in `pyproject.toml`, with permissive version constraints.
 - [ ] Add a `LICENSE` file, if applicable. This is *highly recommended* if the project is open source.
 - [ ] Add a [`CITATION.cff`](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files), to ease citation of your work.
 - [ ] Replace this `README.md` with a proper one. Among others, it must explain the overall context, the installation instructions, a quick start guide, and a repository structure description.


## Getting started

During development, install pinned dependencies in your virtual environment, including the module itself in [editable mode](https://setuptools.pypa.io/en/latest/userguide/development_mode.html), using:
{%- if cookiecutter.use_uv %}

```sh
uv sync
```
{%- else %}

```sh
pip install -r requirements.txt -e . --group dev
```
{%- endif %}

In order to use [pre-commit](https://pre-commit.com/) hooks, they need to be registered:
{%- if cookiecutter.use_uv %}

```sh
uv run pre-commit install
```
{%- else %}

```sh
pre-commit install
```
{%- endif %}

It is a good practice to manually invoke hooks after installation, just in case:
{%- if cookiecutter.use_uv %}

```sh
uv run pre-commit run --all-files
```
{%- else %}

```sh
pre-commit run --all-files
```
{%- endif %}
{%- if cookiecutter.use_uv %}

New dependencies can be added using [`uv add`](https://docs.astral.sh/uv/concepts/projects/dependencies/) (or `uv add --dev` for development dependencies), or by manually configuring `pyproject.toml` and using [`uv lock && uv sync`](https://docs.astral.sh/uv/concepts/projects/sync/).
{%- else %}

New dependencies can be specified directly in `pyproject.toml`; `requirements.txt` will be updated by a pre-commit hook.
{%- endif %}
{%- if cookiecutter.use_pytest %}

Unit tests (using [pytest](https://pytest.org/)) are not executed as a pre-commit hook, to keep the overhead to a minimum.
{%- if cookiecutter.ci_platform != 'none' %}
Instead, a CI/CD pipeline is configured to run tests after each commit.
You can also execute them locally, manually:
{%- else %}
Instead, execute them locally, manually:
{%- endif %}
{%- if cookiecutter.use_uv %}

```sh
uv run pytest
```
{%- else %}

```sh
pytest
```
{%- endif %}
{%- endif %}
