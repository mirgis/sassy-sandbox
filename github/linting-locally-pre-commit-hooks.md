# Motivation

When writing code, it's helpful to ensure it is free of syntax errors and follows established best practices. While you can configure your favorite editor (e.g., [VS Code [1]](https://medium.com/@jackklpan/auto-format-and-lint-by-black-isort-flake8-in-vs-visual-studio-code-a62a3f5d940e)) to run formatters and linters on save, it’s also essential to enforce checks **before code gets committed** (`pre-commit`) or **pushed** (`pre-push`) to a remote repository. Ideally, the code should be verified **at commit time** using a `pre-commit` hook.

# Solution

This guide demonstrates how to enforce Python code quality locally using [`pre-commit`](https://pre-commit.com/) – a framework for managing Git hooks. We’ll configure several powerful tools to automatically run before each commit or push:

- **`isort`** – automatically sorts imports alphabetically and categorizes them by type.
- **`black`** – formats your code to a consistent style based on strict rules.
- **`flake8`** – lints your code for style violations and syntax errors.
- **`mypy`** – performs static type checking for code that uses type annotations.
- **`pylint`** – analyzes Python code to detect programming errors, bugs, and enforce coding standards.

---

### 1. Install Pre-commit and Pylint

Install the core `pre-commit` tool and `pylint` (the only tool run via system Python in this example):

```bash
pip install pre-commit pylint
```

### 2. Create a `.pre-commit-config.yaml` in your project root

```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 25.1.0
    hooks:
      - id: black
        language_version: python3

  - repo: https://github.com/pycqa/isort
    rev: 6.0.1
    hooks:
      - id: isort
        language_version: python3

  - repo: https://gitlab.com/pycqa/flake8
    rev: 7.3.0
    hooks:
    - id: flake8
      language_version: python3

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.17.0
    hooks:
    - id: mypy
      language_version: python3

  - repo: local
    hooks:
      - id: pylint
        name: pylint
        entry: pylint
        language: system
        types: [python]
```
### 3. Enable Pre-commit Hooks
```bash
pre-commit install  # install pre-commit hook (runs before commit)

pre-commit install --hook-type pre-push  # optional: install pre-push hook

pre-commit run --all-files --verbose  # test hooks without commiting or pushing
```

# References
1. [Setup black, isort, flake8 in VSCode](https://medium.com/@jackklpan/auto-format-and-lint-by-black-isort-flake8-in-vs-visual-studio-code-a62a3f5d940e)
2. [Automate Python workflow using pre-commits: black and flake8](https://ljvmiranda921.github.io/notebook/2018/06/21/precommits-using-black-and-flake8/)
3. [Effortless Code Quality: Ultimate Pre-Commit Hooks Guide for 2025](https://gatlenculp.medium.com/effortless-code-quality-the-ultimate-pre-commit-hooks-guide-for-2025-57ca501d9835)
4. [How to use Black and pre-commit for auto text-formatting on commit \[setup\] \[python\]](https://dev.to/emmo00/how-to-setup-black-and-pre-commit-in-python-for-auto-text-formatting-on-commit-4kka)
5. [Using pre-commit hooks for your Python project](https://www.pythonsnacks.com/p/pre-commit-hooks-python)
6. [ChatGPT-4o](https://chat.openai.com/)