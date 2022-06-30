# CU Library Read the Docs

ReadtheDocs is CU Boulder Libraries Core Tech & Apps approved method for documentation. 

1. [Github Markdown Guides](https://guides.github.com/features/mastering-markdown/)
1. [Read the Docs Documentation](https://docs.readthedocs.io/en/stable/index.html)

## Requirements

1. Python 3.3 or greater

## Installation

1. Clone Repository

    ```sh
    git clone git@github.com:culibraries/documentation.git

    or

    git clone https://github.com/culibraries/documentation.git
    ```

1. Create Virtual Environment

    NOTE: Win variations assume cmd.exe shell

    ```sh
    cd documentation
    python3 -m venv venv (Win: python -m venv <dir>)
    . venv/bin/activate (Win: venv\Scripts\activate.bat)
    pip install -r requirements.txt
    ```

1. Create HTML

    ```sh
    cd docs
    make html
    ```

1. New Terminal - Web server

    ```sh
    . venv/bin/activate
    cd docs/_build/html
    python -m http.server
    Serving HTTP on :: port 8000 (http://[::]:8000/) ...
    ```

1. Open Browser [http://localhost:8000](http://localhost:8000)

## Add new documentation

1. `git checkout -b new_docs`
1. Edit/Add documentation (Markdown)
1. `make html`
1. add new pages to toctree (index.rst)

## Pull Request to main branch

CU Boulder Libraries' regular activity is to create a PR from the `release` branch with a code review. The `documentation` repository is slightly different. Perform a PR from the feature branch to the `main` branch. Add a code review before merge to main.

## View Build Process on ReadtheDocs

1. Merge to main required before ReadtheDocs build process will start.
1. [ReadtheDocs View builds](https://readthedocs.org/projects/cu-boulder-libraries/builds/)
1. After successful build: [https://cu-boulder-libraries.readthedocs.io/en/latest/](https://cu-boulder-libraries.readthedocs.io/en/latest/)
