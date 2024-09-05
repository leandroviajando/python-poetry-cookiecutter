# {{ cookiecutter.__directory_name }}

## Development

**Pyenv** and **Poetry** are used to manage Python versions and project dependencies.

### Setup

#### Pyenv and Poetry

1. Install `pyenv` and `pyenv-virtualenv`: e.g. `brew install pyenv pyenv-virtualenv` on Mac, see here for [Linux](https://brain2life.hashnode.dev/how-to-install-pyenv-python-version-manager-on-ubuntu-2004). Note you _may_ have to add the following to your `~/.zshrc` or `~/.bashrc` file:

   ```bash
   eval "$(pyenv init -)"
   eval "$(pyenv virtualenv-init -)"
   ```

2. Create a virtual environment with `pyenv` (or alternatively with `python -m venv .venv`):

   ```bash
   pyenv virtualenv 3.10.12 {{ cookiecutter.__directory_name }}-py3.10.12
   ```

3. Activate the virtual environment and install the required version of Poetry directly in the virtual environment itself - note that at the time of writing, we were using poetry version 1.8.3. This may have changed, so please check `pyproject.toml` to see which version of poetry we are currently using:

   ```bash
   pyenv activate {{ cookiecutter.__directory_name }}-py3.10.12
   ({{ cookiecutter.__directory_name }}-py3.10.12) pip install poetry==1.8.3
   ```

4. Install the project dependencies: (see below for Pre-commit Hooks setup)

   ```bash
   ({{ cookiecutter.__directory_name }}-py3.10.12) make install install-pre-commit
   ```

5. When working with the project always activate the environment first, before running any other commands:

   ```bash
   pyenv activate {{ cookiecutter.__directory_name }}-py3.10.12
   ({{ cookiecutter.__directory_name }}-py3.10.12) make install format
   ```

#### Pre-commit Hooks

For the `Gitleaks` pre-commit hook you need to have `Go` installed on your machine.

You can install it from the [official website](https://golang.org/doc/install), or on Mac also with `brew`:

```bash
brew install golang
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
```
