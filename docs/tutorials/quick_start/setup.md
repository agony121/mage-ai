# Setup and install

You can install Mage using Docker, `pip`, or `conda`.

<br />

## Using Docker

1. Create new project and launch tool

   ```bash
   docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai \
     mage start [project_name]
   ```

    > Windows
    >
    > If you are running Mage in Docker on Windows OS, `$(pwd)` won’t work. Instead, use the following
    > command:
    ```bash
    docker run -it -p 6789:6789 -v "C:\Some Path\To Your Current Directory":/home/src mageai/mageai \
      mage start [project_name]
    ```

2. Open http://localhost:6789 in your browser and build a pipeline.

3. Run pipeline after building it in the tool

   ```bash
   docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai \
     mage run [project_name] [pipeline]
   ```

### Initialize new project

If you want to create a different project with a different name, run the
following:

```bash
docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai \
  mage init [project_name]
```

### Using PostgreSQL as database

Mage uses SQLite as the default database engine. To use PostgreSQL as
the database engine, you'll need to run your Docker container slightly differently:

1. Create Docker network

   ```bash
   docker network create mage-app
   ```

1. Start PostgreSQL Docker container in a separate window

   ```bash
   docker run --network mage-app --network-alias postgres_db \
      -it -p 5432:5432 -v pgdata:/var/lib/postgresql/data \
      -e POSTGRES_USER=<username> -e POSTGRES_PASSWORD=<password> \
      -e POSTGRES_DB=<database> -e PG_DATA=/var/lib/postgresql/data/pgdata \
      postgres:13-alpine3.17 postgres
   ```

1. Launch Mage with `MAGE_DATABASE_CONNECTION_URL` environment variable

   ```bash
   docker run --network mage-app -it -p 6789:6789 -v $(pwd):/home/src \
      -e MAGE_DATABASE_CONNECTION_URL=postgresql+psycopg2://<username>:<password>@postgres_db:5432/<database> \
      mageai/mageai \
      mage start another_repo
   ```

<br />

## Using `pip` or [`conda`](https://github.com/conda-forge/mage-ai-feedstock)

1. Install Mage

   ```bash
   pip install mage-ai
   ```

   or

   ```bash
   conda install -c conda-forge mage-ai
   ```

   <sub>If you run into errors, see the [errors section](#errors) below.</sub>

2. Create new project and launch tool

   ```bash
   mage start [project_name]
   ```

3. Open http://localhost:6789 in your browser and build a pipeline.
4. Run pipeline after building it in the tool

   ```bash
   mage run [project_name] [pipeline]
   ```

### Initialize new project

If you want to create a different project with a different name, run the
following:

```bash
mage init [project_name]
```

<br />

## Download new version of Mage

### Using Docker

```bash
docker pull mageai/mageai:latest
```

### Using `pip`

```bash
pip install -U mage-ai
```

<br />

## Environment variables

If you’re running Mage using `pip` or `conda`, your local machine’s environment variables
are accessible within the running Mage app.

If you’re running Mage using Docker, you must add the following command line flags:

```bash
-e SOME_VARIABLE_NAME_1=secret_value_1 -e SOME_VARIABLE_NAME_2=secret_value_2
```

The command to run Mage using Docker could look like this:

```bash
docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai \
    -e SOME_VARIABLE_NAME_1=secret_value_1 -e SOME_VARIABLE_NAME_2=secret_value_2 \
    mage start demo_project
```

<br />

## Install

### Installing extra packages

Mage also has the following extras:

- **spark**: to use Spark in your Mage pipeline
- **bigquery**: to connect to BigQuery for data import or export
- **hdf5**: to process HDF5 file format
- **postgres**: to connect to PostgreSQL for data import or export
- **redshift**: to connect to Redshift for data import or export
- **s3**: to connect to S3 for data import or export
- **snowflake**: to connect to Snowflake for data import or export
- **all**: to install all of the above to use all functionalities

Example:

```bash
pip install "mage-ai[spark]"
```

<br />

### Errors

You may need to install development libraries for MIT Kerberos to use some Mage
features.

On Ubuntu, this can be installed as:

```bash
apt install libkrb5-dev
```

<br />

## Development environment

To setup a development environment for editing source code, please check out
this [document](/community/contributing).

<br />

## Building your own Docker image

```bash
docker build -t mage/dangerous .
```

<br />

## Testing from branch

Install `mage-ai` library from your branch:

```bash
$ pip3 install git+https://github.com/mage-ai/mage-ai.git@branch_name#egg=mage-ai
```

Build the front-end code:

```bash
$ cd mage_ai/frontend
$ yarn install
$ yarn export_prod
```

<br />

## Testing

### Backend

```bash
$ ./scripts/server/test.sh
```

### Front-end
To check proper types in TypeScript, run:

```bash
$ ./scripts/test.sh
```

<br />
