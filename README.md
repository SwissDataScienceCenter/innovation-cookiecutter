# Cookiecutter templates

This repository contains [Cookiecutter](https://github.com/cookiecutter/cookiecutter) templates for data science projects.

 * [Python - Minimal](./python-minimal/): a simple starting point for a data science project in Python.


## Getting started

If [`uv`](https://docs.astral.sh/uv/) is part of your workflow, you can use [`uvx`](https://docs.astral.sh/uv/guides/tools/) to run Cookiecutter:

```sh
uvx cookiecutter https://github.com/SwissDataScienceCenter/innovation-cookiecutter.git
```

Alternatively, you can install it in your environment and run it directly:

```sh
pip install -U cookiecutter
cookiecutter https://github.com/SwissDataScienceCenter/innovation-cookiecutter.git
```

The interactive command line tool will guide you through the selection and configuration of the template.

You can now create an empty repository on GitHub or GitLab (i.e., without an initial README file), and initialize it locally:

```sh
cd your-project
git init --initial-branch=main
git remote add origin https://github.com/you/your-project.git
git add .
git commit -m "Initial commit"
git push --set-upstream origin main
```

Please refer to the generated `README.md` for more details, in particular to install dependencies and register pre-commit hooks.
