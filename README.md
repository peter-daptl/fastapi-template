# 🚀 FastAPI Project Template (Cookiecutter)

---

This repository provides a robust and opinionated Cookiecutter template for quickly bootstrapping new FastAPI projects. It aims to set up a clean, scalable, and well-structured foundation with modern Python development best practices, including asynchronous capabilities, database integration, and pre-configured tools for linting, formatting, and testing.

---
## ✨ Features of the Generated Project

* **FastAPI:** High-performance, easy to use, web framework for building APIs.
* **SQLAlchemy (Async):** Asynchronous ORM for database interactions.
* **Pydantic:** Data validation and settings management.
* **Pytest & Pytest-Cov:** Comprehensive testing framework with code coverage reporting.
* **Black:** Uncompromising code formatter.
* **isort:** Sorts imports automatically.
* **Flake8:** Pluggable linting tool for PEP 8 compliance and more.
* **Modular Structure:** Organized into `api`, `schemas`, `database`, `config`, etc., for maintainability.
* **FastAPI Dependency Injection:** Structured use of FastAPI's dependency system.
* **Basic API Versioning (Optional):** Setup for `api/v1` routes.

---

## ⚡ How to Use This Template

To use this template, you need `cookiecutter` installed. If you don't have it, install it via pip:

```bash
pip install cookiecutter
cookiecutter https://github.com/peter-daptl/fastapi-template.git
```

Cookiecutter will then prompt you for several project-specific details. Answer these questions to customize your new FastAPI project.

---
## After generation:

### Note - A separate README will be generated in your template also summarizing these notes.

1.  **Navigate into your new project directory:**
    ```bash
    cd your_project_name/ # Replace with the name you provided during prompts
    ```
2.  **Set up the virtual environment and install dependencies:**

    Depending on the option you selected during project generation, choose the appropriate setup method below:

    a.  **Using `venv` (Standard Python Virtual Environment):**
    ```bash
        python3 -m venv venv
        # Activate for Linux/macOS:
        source venv/bin/activate
        # Activate for Windows (Command Prompt):
        # venv\\Scripts\\activate.bat
        # Activate for Windows (PowerShell):
        # .\\venv\\Scripts\\Activate.ps1

        pip install -r requirements.txt
    ```

    b.  **Using `Pipenv`:**
        *If you don't have Pipenv installed globally:*
    ```bash
        pip install pipenv
    ```
    *Then, in your project directory:*

    ```bash
        pipenv install --dev --skip-lock
        pipenv shell
    ```

    c.  **Using `Poetry`:**
        *If you don't have Poetry installed globally (recommended via official installer):*
    ```bash
        curl -sSL [https://install.python-poetry.org](https://install.python-poetry.org) | python3 -
        # Follow prompts to add Poetry to your PATH
    ```
       *Then, in your project directory:*
    ```bash
        poetry install --with dev
        poetry shell
    ```

3.  **Run tests (and check coverage):**
    ```bash
    pytest --cov=.
    ```
4.  **Run linting & formatting checks:**
    ```bash
    flake8 .
    isort . --check-only --diff
    black . --check --diff
    ```
5.  **Start the development server:**
    ```bash
    uvicorn main:app --reload
    ```
    Your API will be available at `http://127.0.0.1:8000`.

---
## 🛡️ Pre-commit Hooks

Projects generated from this template utilize [Pre-commit](https://pre-commit.com/) hooks to automatically check and format your code before every commit. This helps maintain consistent code style, catches common errors early, and ensures code quality across the team.

### Installation & Setup

1.  **Install `pre-commit`** (if you don't have it already):
    ```bash
    pip install pre-commit
    ```
2.  **Install the Git hooks** into your repository (from the project's root directory):
    ```bash
    pre-commit install
    ```
    Once installed, the hooks will automatically run every time you attempt to `git commit`. If a hook fails, the commit will be aborted, allowing you to fix the issues before committing.

### Configured Hooks

The following hooks are configured in `.pre-commit-config.yaml` and will run on your code:

* **`trailing-whitespace`**: Removes extra whitespace at the end of lines.
* **`end-of-file-fixer`**: Ensures that files end with a newline and removes any excess newlines at the end of files.
* **`check-yaml`**: Checks YAML file syntax.
* **`check-added-large-files`**: Prevents accidentally adding very large files to the repository.
* **`debug-statements`**: Catches common debug statements (`print`, `breakpoint`, `ipdb.set_trace`, etc.) that might be left in production code.
* **`requirements-txt-fixer`**: Automatically sorts and de-duplicates entries in `requirements.txt`.
* **`black`**: Formats Python code to adhere to the Black style guide (`--line-length=100`).
* **`isort`**: Sorts Python import statements (`--profile=black`, `--line-length=100`).
* **`flake8`**: Lints Python code to enforce style consistency and detect potential errors (`--max-line-length=100`, `--ignore=E203,W503` to avoid conflicts with Black).
* **`autoflake`**: Automatically removes unused imports and unused variables from Python code (`--in-place`, `--remove-unused-variables`, `--remove-all-unused-imports`).
* **`pytest` (local hook)**: Runs all your project's tests (`pytest`). This hook is configured to `always_run`, meaning tests will execute on every commit, regardless of which files changed. This is a crucial safety measure to prevent committing breaking changes.

---

## ⚙️ Cookiecutter Prompts Explained

When you run `cookiecutter`, you'll be asked a series of questions. Here's what each prompt does:

| Prompt Key (`cookiecutter.json` variable) | Description                                                                                             | Effect on Generated Project                                                                                                                  |
| :---------------------------------------- | :------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------- |
| `project_name`                            | The human-readable title for your project.                                                              | Used in `README.md`, documentation, and potentially other generated files.                                                                   |
| `project_slug`                            | A URL- and Python-friendly version of your project name, automatically generated.                       | Forms the root directory name of the generated project and is used for internal naming conventions.                                          |
| `app_name`                                | The main Python package name for your application (e.g., `my_app`). Must be a valid Python identifier. | Used as the top-level Python import path (`from {{ cookiecutter.app_name }} import ...`).                                                  |
| `db_url`                                  | The SQLAlchemy database connection string for your application.                                         | Configures your application's database connection URL in settings. Common examples: `sqlite+aiosqlite:///./data/app.db` or `postgresql+asyncpg://user:password@host/db_name`. |
| `use_pipenv`                              | Choose `y` to configure the project to use Pipenv for dependency management. (`y`/`n`)                 | If `y`, `Pipfile` and `Pipfile.lock` are generated. If `n`, `requirements.txt` is used.                                                      |
| `use_poetry`                              | Choose `y` to configure the project to use Poetry for dependency management. (`y`/`n`)                 | If `y`, `pyproject.toml` is configured for Poetry. If `n`, other dependency managers are used. (Note: Only one of `use_pipenv` or `use_poetry` should typically be 'y'). |
| `include_example_item_crud`               | Include a basic example for Item CRUD (Create, Read, Update, Delete) operations and related tests? (`y`/`n`) | If `y`, generates example `api/v1/items` views, services, schemas, and corresponding tests. Provides a quick start for new resources.          |

---


## ⚠️ Important Notes & Disclaimer

* **Option Combinations:** While efforts are made to ensure all options work together, **not all possible combinations of choices have been exhaustively tested**. If you encounter issues with a specific set of options, please report it!
* **Database Migrations:** This template provides the ORM setup but does **not** include an Alembic (or similar) migration tool setup by default. You will need to integrate a migration tool if your project requires schema evolution.
* **Further Customization:** This is a starting point. Feel free to extend, remove, or modify any part of the generated project to fit your specific needs.

---

## 🤝 Contributing to This Template

If you have suggestions, bug fixes, or improvements for this **Cookiecutter template itself**, please feel free to open an issue or submit a pull request on the [template's GitHub repository]([YOUR_REPO_URL]). Your contributions are welcome!

---
