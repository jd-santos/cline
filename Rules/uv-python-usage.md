---
description: A concise guide for LLMs on using UV for Python project management, focusing on core commands and configuration.
author: JD
version: 2.0
tags: ["uv", "python", "package-manager", "venv", "guide", "llm"]
globs: ["**/*.py", "pyproject.toml"]
---

# Using UV for Python Projects

UV is a fast Python package manager and resolver. This guide provides the essential commands and configuration for an LLM to manage Python projects using UV. For more details, refer to the [official UV documentation](https://astral.sh/docs/uv).

## Core Concepts

-   **Project-based**: UV operates on projects defined by a `pyproject.toml` file
-   **Locking**: Use `uv.lock` for reproducible environments. Create it with `uv lock` and install from it with `uv sync`
-   **Virtual Environments**: UV automatically creates and uses a virtual environment in `.venv`. Activate it with `source .venv/bin/activate`

## Common Commands

### Project and Dependencies

-   **Initialize a project**
    ```bash
    uv init
    ```
-   **Add a dependency**
    ```bash
    uv add <package>
    uv add "flask<3"
    uv add --dev pytest
    ```
-   **Remove a dependency**
    ```bash
    uv remove <package>
    ```
-   **Update the lockfile**
    ```bash
    uv lock
    ```
-   **Install dependencies from `uv.lock`**
    ```bash
    uv sync
    ```

### Running Commands

-   **Run a command in the project environment**
    ```bash
    uv run <command>
    # e.g., uv run python main.py
    # e.g., uv run pytest
    ```
-   **Run a tool without installing it**
    ```bash
    uvx <command>
    # e.g., uvx black .
    ```

### Pip-Compatible Commands

UV can be used as a drop-in replacement for `pip`.

-   **Install packages**
    ```bash
    uv pip install <package>
    uv pip install -r requirements.txt
    uv pip install -e .
    ```
-   **Uninstall packages**
    ```bash
    uv pip uninstall <package>
    ```
-   **List installed packages**
    ```bash
    uv pip list
    ```
-   **Generate `requirements.txt`**
    ```bash
    uv pip freeze > requirements.txt
    ```

### Python Version Management

-   **Install a Python version**
    ```bash
    uv python install <version>
    # e.g., uv python install 3.12
    ```
-   **List available/installed Pythons**
    ```bash
    uv python list
    ```
-   **Find a Python executable**
    ```bash
    uv python find
    ```

## Configuration (`pyproject.toml`)

UV is configured in the `pyproject.toml` file.

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "A short description."
requires-python = ">=3.9"
dependencies = [
    "requests>=2.28.0",
    "flask[async]>=2.0.0",
]

[project.optional-dependencies]
test = ["pytest>=7.0"]
dev = ["black", "ruff"]

[tool.uv]
# See https://astral.sh/docs/uv/configuration
```

## Further Reading

-   [Installation](https://astral.sh/docs/uv/installation)
-   [Project Management](https://astral.sh/docs/uv/projects)
-   [Python Management](https://astral.sh/docs/uv/python)
-   [Configuration](https://astral.sh/docs/uv/configuration)
