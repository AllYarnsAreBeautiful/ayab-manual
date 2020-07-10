# AYAB Manual

This manual is [mkdocs](https://www.mkdocs.org/) based.
It supports documentation of multiple versions using the
[mike](https://pypi.org/project/mike/) utility.

## Install mkdocs and mike

You might want to work in a Python virtualenv:

```bash
virtualenv --python=/usr/bin/python3 venv/
source venv/bin/activate
python -m pip install -r requirements.txt
```

## View documentation locally

`mike serve`

This command starts a local webserver at [localhost:8000](http://localhost:8000).

## Add a new version

`mike deploy <version> <alias> <-u>`

`<version>` is the version number of the documentation (e.g. 1.0.0).  
`<alias>` is an alias for this version. Currently, we use the alias `latest`
to set the version which is automatically shown when visiting the website.

If the version number (e.g. 1.0.0) already exists, you have to add `-u` (for
update) to the command.

## Deploy to Github Pages

To push the content to Github, add `-p`.

This will build the static files and push to the gh-pages branch on Github,
so they will be served automatically at
[manual.ayab-knitting.com](http://manual.ayab-knitting.com).

So a complete command might look like this:

`mike deploy 0.95 latest -u -p`
