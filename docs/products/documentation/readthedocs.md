# CU Library Read the Docs

ReadtheDocs is CU Boulder Libraries Core Tech & Apps approved method for documentation. 

[Github Markdown Guides](https://guides.github.com/features/mastering-markdown/)

[Read the Docs Documentation](https://docs.readthedocs.io/en/stable/index.html)

## Requirements

1. Python 3.3 or greater

## Installation (Mac OS)

1. Clone Repository 

        $ git clone git@github.com:culibraries/documentation.git

        or

        $ git clone https://github.com/culibraries/documentation.git

2. Create Virtual Environment


        $ cd documentation

        $ python3 -m venv venv

        $ . venv/bin/activate

        $ pip install -r requirements.txt

3. View Documentation

        $ cd docs
        $ make html

4. New Terminal - Web server

        $ . venv/bin/activate
        $ cd docs/_build/html
        $ python -m http.server
        Serving HTTP on :: port 8000 (http://[::]:8000/) ...

5. Open Browser [http://localhost:8000](http://localhost:8000)

## Add new Documentation

1. git checkout -b new_docs
2. Edit add documentation (Markdown)
3. make html 
4. add new pages to toctree (index.rst)


## Pull Request main <<--- feature branch

CU Boulder Libraries' regular activity is to create a PR from the Release branch with a code review. Our Documentation repository is slightly different. Perform a PR from the feature branch to the main branch. Add a code reviewer before merge to main.

## View Build Process on ReadtheDocs

1. Merge to main required before ReadtheDocs build process will start.
2. [View builds](https://readthedocs.org/projects/cu-boulder-libraries/builds/)
3. After successful build ==>> [https://cu-boulder-libraries.readthedocs.io/en/latest/](https://cu-boulder-libraries.readthedocs.io/en/latest/)