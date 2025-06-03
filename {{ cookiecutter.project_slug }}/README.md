# {{ cookiecutter.project_name }}

This is a Cookiecutter template for generating a FastAPI project with the following features:

* **True Class-Based Views:** It uses `fastapi-utils`'s `@cbv` decorator for a clean, object-oriented API structure.
* **Modular Design:**
    * Pydantic schemas are centralized in `schemas/`.
    * SQLAlchemy ORM models are in `database/models/`.
    * Views and service (domain) logic are organized into feature-specific folders (e.g., `api/v1/items/`), reflecting the API routes.
    * **Strict Import Style:** Uses fully qualified imports (e.g., `from {{ cookiecutter.app_name }}.some_module.sub_module import ...`) for clarity. `__init__.py` files declare their public API using `__all__` to explicitly manage re-exports.
* **Google-Style Docstrings:** All Python code uses Google-style docstrings for consistent and clear documentation.
* **Separation of Concerns:** Service layer (your "domain" logic) explicitly uses Pydantic schemas for input/output, ensuring consistency between the API layer and business logic.
* **Pre-Commit Hooks:** Configured with essential pre-commit hooks for code quality and consistency:
    * `trailing-whitespace`
    * `end-of-file-fixer`
    * `check-yaml`
    * `check-added-large-files`
    * `debug-statements` (removes `print` and `breakpoint` calls)
    * `black` (code formatter)
    * `flake8` (linter)
    * `isort` (import sorter)
    * `autoflake` (removes unused imports and variables)
    * **`pytest`**: Ensures all tests pass before committing.
* **Testing Setup:** Includes a `tests/` directory with a sample `pytest` test file and `httpx` for API testing.
* **Flexible Dependency Management:** Includes `requirements.txt`, `Pipfile` (if `use_pipenv` selected), and `pyproject.toml` to support `pip` (with `venv`), `pipenv`, or `poetry` workflows.
* **Centralized Tool Configuration:** `pyproject.toml` contains configuration for `black`, `isort`, and `flake8`, ensuring consistency across your development environment.
* **`.gitignore`:** Pre-configured to ignore common Python, Git, and IDE artifacts, including lock files for different package managers.

---

## **How to Use**

To get started with your new project, follow these steps:

1.  **Install Cookiecutter:**
    ```bash
    pip install cookiecutter
    ```

2.  **Generate a new project:**
    Navigate to the directory where you want your new project to be created, then run:
    ```bash
    cookiecutter /path/to/this/template/fastapi-class-based-template
    ```
    (Remember to replace `cookiecutter /path/to/this/template/fastapi-class-based-template` with the actual path to where you've saved this Cookiecutter template.)

3.  **Follow the prompts** to name your project.

4.  **Navigate into your new project directory:**
    ```bash
    cd {{ cookiecutter.project_slug }}
    ```

5.  **Choose your preferred package manager and set up the environment:**

    ### Option 1: Using `pip` (with `venv`)

    This is the standard Python approach.

    ```bash
    python -m venv .venv
    source .venv/bin/activate  # On Windows: .venv\Scripts\activate
    pip install -r requirements.txt
    pip install pre-commit
    pre-commit install
    ```

    ### Option 2: Using `pipenv`

    `pipenv` offers a simpler workflow for managing virtual environments and dependencies.
    *{% if cookiecutter.use_pipenv == "y" %}*
    ```bash
    pip install pipenv
    pipenv install --dev
    pipenv run pre-commit install
    ```

    **To work within the pipenv shell:**
    Once installed, you can enter the project's isolated virtual environment. Commands you run directly will then use the packages installed by pipenv without needing `pipenv run` every time.
    ```bash
    pipenv shell
    # Now you can run commands directly, e.g.:
    # uvicorn {{ cookiecutter.app_name }}.main:app --reload
    # pytest
    # Type 'exit' or Ctrl+D to leave the pipenv shell
    ```
    *{% else %}*
    *(Pipenv option was not selected during project generation.)*
    *{% endif %}*

    ### Option 3: Using `poetry`

    `poetry` is a modern dependency manager and packaging tool that simplifies project setup.
    *{% if cookiecutter.use_poetry == "y" %}*
    ```bash
    pip install poetry
    poetry install --with dev
    poetry run pre-commit install
    ```

    **To work within the poetry shell:**
    Once installed, you can enter the project's isolated virtual environment. Commands you run directly will then use the packages installed by poetry without needing `poetry run` every time.
    ```bash
    poetry shell
    # Now you can run commands directly, e.g.:
    # uvicorn {{ cookiecutter.app_name }}.main:app --reload
    # pytest
    # Type 'exit' or Ctrl+D to leave the poetry shell
    ```
    *{% else %}*
    *(Poetry option was not selected during project generation.)*
    *{% endif %}*

---

## **Running the Application and Tests**

### Running the FastAPI Application

* **With `pip` (after `venv` activated):**
    ```bash
    uvicorn {{ cookiecutter.app_name }}.main:app --reload
    ```
* **With `pipenv`:**
    ```bash
    pipenv run uvicorn {{ cookiecutter.app_name }}.main:app --reload
    # OR: If you are in the `pipenv shell`, just `uvicorn {{ cookiecutter.app_name }}.main:app --reload`
    ```
* **With `poetry`:**
    ```bash
    poetry run uvicorn {{ cookiecutter.app_name }}.main:app --reload
    # OR: If you are in the `poetry shell`, just `uvicorn {{ cookiecutter.app_name }}.main:app --reload`
    ```

Your API will be accessible at `http://127.0.0.1:8000` and the interactive OpenAPI documentation at `http://127.0.0.1:8000/docs`.

### Running Tests

While `pre-commit` will automatically run your tests before each commit, you can also run them manually:

* **With `pip` (after `venv` activated):**
    ```bash
    pytest
    ```
* **With `pipenv`:**
    ```bash
    pipenv run pytest
    # OR: If you are in the `pipenv shell`, just `pytest`
    ```
* **With `poetry`:**
    ```bash
    poetry run pytest
    # OR: If you are in the `poetry shell`, just `pytest`
    ```

---

## **Generating API Documentation with Sphinx**

This template uses **Google-style docstrings**, which are highly recommended for readability and compatibility with documentation generation tools. You can easily generate beautiful API documentation using [Sphinx](https://www.sphinx-doc.org/en/master/) along with its extensions.

To do this, you would typically follow these general steps:

1.  **Install Sphinx and Napoleon:**
    You'll need `sphinx` itself and the `sphinx.ext.napoleon` extension, which allows Sphinx to understand Google-style docstrings.
    ```bash
    pip install sphinx sphinx_rtd_theme sphinx.ext.autodoc sphinx.ext.napoleon
    ```
    (You might also want `sphinx_rtd_theme` for a good-looking theme).

2.  **Initialize Sphinx:**
    From your project's root directory, run `sphinx-quickstart` and follow the prompts. When asked, ensure you enable the `autodoc` and `napoleon` extensions.
    ```bash
    sphinx-quickstart
    ```
    This will create a `docs/` directory with basic Sphinx configuration files (e.g., `conf.py`, `index.rst`).

3.  **Configure `conf.py`:**
    In `docs/conf.py`, you'll need to:
    * Add your project's root directory to the Python path so Sphinx can find your modules:
        ```python
        import os
        import sys
        sys.path.insert(0, os.path.abspath('..')) # Adjust if your docs are not in docs/
        ```
    * Ensure `sphinx.ext.autodoc` and `sphinx.ext.napoleon` are in your `extensions` list:
        ```python
        extensions = [
            'sphinx.ext.autodoc',
            'sphinx.ext.napoleon',
            # Add other extensions as needed, e.g., 'sphinx.ext.viewcode'
        ]
        ```

4.  **Create RST files for your API documentation:**
    You can create `.rst` files (e.g., `docs/api.rst`) to include your API documentation. Use `automodule` and `autoclass` directives to pull docstrings directly from your Python code:
    ```rst
    API Reference
    =============

    .. automodule:: {{ cookiecutter.app_name }}
        :members:

    .. automodule:: {{ cookiecutter.app_name }}.api.v1.items.views
        :members:

    .. automodule:: {{ cookiecutter.app_name }}.api.v1.items.services
        :members:

    .. automodule:: {{ cookiecutter.app_name }}.database.models.item
        :members:
    ```

5.  **Build your documentation:**
    From inside your `docs/` directory, run:
    ```bash
    make html
    ```
    This will generate HTML documentation in `docs/_build/html/`.

These steps provide a quick overview. For detailed configuration and advanced features, refer to the [Sphinx documentation](https://www.sphinx-doc.org/en/master/contents.html).
