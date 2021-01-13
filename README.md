# Data Harvest Specification

Stream dataset repository contains everything to point to data source and metadata about the data.

URL example: https://github.com/ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered

```
Root:
    - meta.yml
    - config.yml
    - history/
        - process.yml
        - request.yml
        - provenance.json
        - response.json
    - recipes/
        - pipeline.py
    - requirements.txt
    - .github/
        - workflows/
        - ISSUE_TEMPLATE/
```

NOTE: To allow for skipping ci with `[skip ci]` in commit message
```
name: Build and publish
runs-on: ubuntu-18.04
if: "!contains(github.event.head_commit.message, '[skip ci]')
```

---
### meta.yml

Specification:

```yaml
data_stream: CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered
version: 2021.01.07
last_updated: 2021-01-07T23:30:55.159Z
last_archived: ""
start_date: 2014-11-03T22:08:04.000Z
end_date: 2020-08-18T01:23:18.820Z
data_location: s3://ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered
archive_location: ""
source_provenance: https://raw.githubusercontent.com/ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered/main/history/provenance.json
```

---
### config.yml

Specification:

```yaml
refresh: false
schedule: "0 0 * * *" # daily at midnight (Overridden with refresh is true)
data_bucket: s3://ooi-data
```

When refresh is true, a trigger should happen to kick off the request and data harvesting process, though process will not happen unless there is a change to the `request.yml` file.

---
### history/request.yml

`request.yml` is the history of data requests to UFrame

Specification:

```yaml
data_stream: CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered
status: success
data_ready: true
last_request: 2021-01-07T23:30:55.159Z
response: https://raw.githubusercontent.com/ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered/main/history/response.json
```

status: can be `success`, `pending`, `failed`

---
### history/process.yml

`process.yml` is the history of data processing...

Specification:

```yaml
data_stream: CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered
data_location: s3://ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered
status: success
last_updated: 2021-01-07T23:30:55.159Z
start_date: 2014-11-03T22:08:04.000Z
end_date: 2020-08-18T01:23:18.820Z
```

The history log of changes can be found at https://github.com/ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered/commits/main/history.yml

See how all the commit updates work here: https://github.com/upptime/uptime-monitor/blob/master/src/update.ts

Example of history view: https://github.com/cormorack/service-status/commits/master/history/interactive-oceans-website.yml

---
### !!! Failed Harvest
When a harvest fails, change `process.yml` status to failed. Then open a new issue in repo.
Example https://github.com/cormorack/service-status/issues/1

```yaml
title: Harvest Processing failed at 2021-01-07T23:30:55.159Z
body: "In https://github.com/ooi-data/CE01ISSM-MFD35-02-PRESFA000-recovered_host-presf_abc_dcl_tide_measurement_recovered/commit/5b8c9615c06dc909091011948999a3c385db8c39. Harvest failed. {exception here}"
labels:
    - failed
    - harvest
    - status
assignee: lsetiawan
```

Use pygithub: https://pygithub.readthedocs.io/en/latest/examples/Issue.html
or have a final prefect github issue task in pipeline https://docs.prefect.io/api/latest/tasks/github.html#opengithubissue
