# ILRI Data Portal Migration
A workflow using [ckanapi-exporter](https://github.com/ckan/ckanapi-exporter) to export metadata from ILRI's CKAN Data Portal to CSV for migration to other data repositories. Right now it is unclear whether we'll be aligning to Dataverse's own metadata or some other schema like DDI.

## Setup
If you have [poetry](https://python-poetry.org/) installed you can set up and activate the environment like this:

```console
$ poetry install
$ poetry shell
```

Otherwise you will have to create the Python virtual environment and install the requirements manually:

```console
$ python2 -m virtualenv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

## Running
To export CKAN metadata into CSV format aligning with Dataverse metadata fields:

```console
$ ckanapi-exporter --url https://data.ilri.org/portal --columns columns-ckan2dataverse.json > output.csv
```

## License
This work is licensed under the [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html).

The license allows you to use and modify the work for personal and commercial purposes, but if you distribute the work you must provide users with a means to access the source code for the version you are distributing. Read more about the [GPLv3 at TL;DR Legal](https://tldrlegal.com/license/gnu-general-public-license-v3-(gpl-3)).
