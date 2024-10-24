# APPN-Template

This repository provides a package template with the following default tools:

- [PDM](https://pdm-project.org/en/latest/) as the dependency manager
- [Ruff](https://github.com/astral-sh/ruff) as the linter and code formatter
- [Mypy](https://mypy.readthedocs.io/en/stable/index.html) as the static type checker
- [PDM-Backend](https://backend.pdm-project.org/) as the build backend (for building wheel distribution)
- [PDM-Bump](https://pypi.org/project/pdm-bump/) as the plugin for bumping version.
- [Pre-commit](https://pre-commit.com/) as precommit hook.
- [pyproject.toml](https://peps.python.org/pep-0518/) for storing project metadata and tool config.
- [pytest](https://docs.pytest.org/en/stable/) for unit testing.
- [pytest-cov](https://pypi.org/project/pytest-cov/) for generating coverage reports.
- `Github-Action` as the CI runner with default CIs for code validation and testing.
- `Makefile` as local CI

Pre-configured options for tools like `ruff`, `mypy`, `pytest` and `pytest-cov` are defined in `pyproject.toml`, under the corresponding table - i.e. `[tools.mypy]`.

# Commands cheat sheet

## Installing PDM
The following commands require you having at least `pdm` in your current environment. For the best developer experience, I suggest installing [pipx](https://github.com/pypa/pipx?tab=readme-ov-file#install-pipx) then installing `pdm` using this command:

```bash
pipx install pdm
```

This should make `pdm` available to your global environment -i.e. it exposes the name `pdm` to `PATH` so now you can run `pdm` as a CLI tool like `grep`. It is a set and forget thing.

## Updating PDM

If you already have pdm but you want to update to the latest version:

```bash
pdm self update
```

## New project from a template

```bash
pdm init <path_to_template>
```

For instance, to use this project as template:

```bash
pdm init https://github.com/harryle95/pdm-template
```

## Managing virtual environment

By default when running the previous command, pdm will create a `.venv` folder in your current project that has the python version matching `requires-python` in `pyproject.toml` (3.11 as per this template).

### Creating a different virtual environment/different python version

Run this command:

```bash
pdm venv create --name <your_venv_name> <python_version_number>
```

Specify the version number like `3.12` or `3.11.8`. PDM will download the best-matching CPython interpreter for the environment. Make sure this version is compatible with `requires-python` field inside `pyproject.toml`.

### Activating virtual environment

For the virtual environment `.venv` in your current project:

```bash
source .venv/bin/activate
```

For the pdm created virtual environment:

```bash
eval $(pdm venv activate <your_env_name>)
```

Running `pdm venv activate <your_env_name>` simply returns the command you can use the activate the venv. To actually activate it, you will need to run in a sub shell and eval with `eval`.

### Using a non-pdm virtual environment
If you want to use your python interpreter instead of the one pdm uses - i.e with `pyenv` or some other tools, feel free to delete the default `.venv` folder then run the following command:

```bash
pdm use -f path/to/my/venv
```

## Adding and removing packages

### To/From the global dependency group

```bash
pdm add <dependency>
```

```bash
pdm remove <dependency>
```

### To/From a development group:

```bash
pdm add -dG <group_name> <dependency>
```

```bash
pdm remove -dG <group_name> <dependency>
```

For instance, to add `mypy` and `ruff` to the developer dep group for validation:

```bash
pdm add -dG validation mypy ruff
```

## Publishing your package

For more information, please visit the official documentation [page](https://pdm-project.org/latest/usage/publish/)

### Configuring credentials

Configuring publishing repository:

```bash
pdm config repository.<name>.url <your_url>
```

Configuring username and password

```bash
pdm config repository.<name>.username <your_name>
pdm config repository.<name>.password <your_password>
```

`name` here can be `pypi`, `testpypi` or any repository that conforms to [PEP 503](https://peps.python.org/pep-0503/). If your repository is `pypi`, `your_name` is `__token__` and `your_password` is the token key generated in pypi portal.

### Bumping version

This requires having [pdm-bump](https://pypi.org/project/pdm-bump/) plugin. You can either use this tool or any other tool of choice (or even manually changing the version yourself) to update your package version and avoid conflict with pypi existing repo.

### Generating build distribution

Running the following command will generate both `sdist` and `wheel` of your package, which suits me 100% of the time. For more information and other arguments, please visit the official CLI [reference](https://pdm-project.org/en/latest/reference/cli/#build)

```bash
pdm build
```

### Publishing

If you have already configured pypi username and password, publishing to pypi is as simple as

```bash
pdm publish
```

Alternatively, if you have configured a different repo url, username and password, you can publish to that repo using:

```bash
pdm publish <repo_name>
```

## CI

### Debug locally

Run

```bash
make check-all
```
