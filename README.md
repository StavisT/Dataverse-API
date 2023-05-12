
`dataverse-api`
================================
[![Build Status](https://github.com/MarcusRisanger/dataverse-api/workflows/release/badge.svg)](https://github.com/MarcusRisanger/dataverse-api/actions)
<!-- [![codecov](https://codecov.io/gh/THREE60-Energy/rushmore-tools/branch/main/graph/badge.svg)](https://codecov.io/gh/THREE60-Energy/rushmore-tools) -->
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)

The `dataverse-api` package is an abstraction layer developed for allowing simple interaction with Microsoft Dataverse Web API.

Overview
================================
The main goal of this project was to allow for simple upserts and inserts of data into Dataverse tables using simple and ubiquitous data structures, with use of batch requests to avoid frequent hits on the REST API. It is based on Python 3.9 to be compatible with current Python runtimes in Azure Functions.

Usage is fairly simple and assumes that a valid app registration for writing to Dataverse exists:

```
import os
from dataverse_api import DataverseClient

app_id = os.environ["app_id"]
authority_url = os.environ["authority_url"]
client_secret = os.environ["client_secret"]
url = os.environ["url"]
scopes = [url + "/.default"]


client = DataverseClient(
    app_id=app_id,
    client_secret=client_secret,
    authority_url=authority_url,
    scopes=scopes,
    dynamics_url=url,
    validate=True,
)
table = client.entity("xyz_my_table")

data = [
    {"xyz_my_table_key": "Foo", "xyz_my_table_col": 1010},
    {"xyz_my_table_key": "Bar", "xyz_my_table_col": 1020},
]

table.upsert(data)
```

## Development environment

We use [poetry](https://python-poetry.org) to manage dependencies and to administrate virtual environments. To develop
`dataverse-api`, follow the following steps to set up your local environment:

 1. [Install poetry](https://python-poetry.org/docs/#installation) if you haven't already.

 2. Clone repository:
    ```
    $ git clone git@github.com:MarcusRisanger/dataverse-api.git
    ```
 3. Move into the newly created local repository:
    ```
    $ cd dataverse-api
    ```
 4. Create virtual environment and install dependencies:
    ```
    $ poetry install
    ```

### Code requirements

All code must pass [black](https://github.com/ambv/black) and [isort](https://github.com/timothycrosley/isort) style
checks to be merged. It is recommended to install pre-commit hooks to ensure this locally before commiting code:

```
$ poetry run pre-commit install
```

Each public method, class and module should have docstrings. Docstrings are written in the [Google
style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings).

### Testing

To produce Coverage tests, run the following commands
```
$ poetry run coverage run -m pytest
$ poetry run coverage xml
```
