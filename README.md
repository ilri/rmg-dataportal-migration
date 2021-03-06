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

*N.B.*: The pattern specification in columns.json is very close to the structure of the JSON output from the CKAN API. See below for reference.

## TODO
Some questions we need to answer:

- How to handle some overlapping and ambiguous metadata fields in CKAN:
  - `title` vs `ILRI_prjtitle`
  - `ILRI_prjstaff` vs `ILRI_actystaff`
  - `ILRI_prjpartners` vs `ILRI_actypartners`
  - `ILRI_prjregions` vs `ILRI_actyregions` (strange that `ILRI_actyregions` don't appear in PostgreSQL keys, only in REST API responses!)
  - `ILRI_prjcountries` vs `ILRI_actycountries`
  - `ILRI_prjpi` vs `ILRI_actypi`
- How to handle multi-value fields?
  - Countries seem to use `+`
  - Project partners seem to use `;`
  - Authors seem to use `,`

## Notes
Some notes about extracting data from CKAN using the API:

Get a list of packages:

```
$ curl https://data.ilri.org/portal/api/3/action/package_list
```

Dump datasets in one package using the `ckanapi` client:

```
$ ckanapi dump datasets restoration-of-degraded-land -r https://data.ilri.org/portal --datapackages=./dump_directory/
```

Show JSON representation of one package (dataset) using `ckanapi` client:

```
$ ckanapi action package_show id=restoration-of-degraded-land -r https://data.ilri.org/portal
```

See more of CKAN's metadata fields in the `package` and `package_extra` tables in PostgreSQL:

- Vanilla CKAN fields: `ckan=> SELECT * FROM package LIMIT 50;`
- Custom ILRI fields:

```
ckan=# SELECT DISTINCT key FROM package_extra ORDER BY key;
...
 ILRI_actyboundbox
 ILRI_actyboundboxcenter
 ILRI_actycitation
 ILRI_actycitationacknowledge
 ILRI_actycontactemail
 ILRI_actycontactperson
 ILRI_actycustodian
 ILRI_actycustodianemail
 ILRI_actydataowner
 ILRI_actydatavailable
 ILRI_actydatecollected
 ILRI_actydatecollectedend
 ILRI_actyfarmconsent
 ILRI_actyipownership
 ILRI_actymapextent
 ILRI_actymapzoom
 ILRI_actynatlevel
 ILRI_actyotheruse
 ILRI_actypartners
 ILRI_actypi
 ILRI_actypiemail
 ILRI_actyprojection
 ILRI_actyrelconfdata
 ILRI_actyresolution
 ILRI_actysharingagreement
 ILRI_actystaff
 ILRI_actyusageconditions
 ILRI_crpandprogram
 ILRI_prjabstract
 ILRI_prjcountries
 ILRI_prjdonor
 ILRI_prjedate
 ILRI_prjgrant
 ILRI_prjpartners
 ILRI_prjpi
 ILRI_prjpiemail
 ILRI_prjregions
 ILRI_prjsdate
 ILRI_prjspecies
 ILRI_prjstaff
 ILRI_prjtitle
 ILRI_prjwebsite
```

## License
This work is licensed under the [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html).

The license allows you to use and modify the work for personal and commercial purposes, but if you distribute the work you must provide users with a means to access the source code for the version you are distributing. Read more about the [GPLv3 at TL;DR Legal](https://tldrlegal.com/license/gnu-general-public-license-v3-(gpl-3)).
