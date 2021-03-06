# voting-record

Voting-record is a web app for tracking Portland, Maine city council votes.

## Data sources

Votes back as far as 2014 appear to be recorded in PDF's listed under City
Council on http://www.portlandmaine.gov/AgendaCenter

Some record of orders as far back as fiscal year 2012/2013 (FY 12/13) can
be found in PDF's listed under City Council on
http://www.portlandmaine.gov/DocumentCenter

## Contributing

### Dependencies

* [Node v6.10.3 (other versions may work)](https://github.com/creationix/nvm)
* [Docker](https://www.docker.com/docker-mac)

### Setup

```sh
npm install
```

### Local development

We don't use `package.json` scripts because JSON doesn't allow comments.

To run the development servers (Postgres):

```sh
docker-compose up
```

You can shut them down with Ctrl+C.

To initialize the database:

```
./sql.sh sql/encoding.sql
# DID THAT SAY UTF8? IF NOT, GET IT FIXED!
./sql.sh sql/councilors.sql
```

To run database commands manually....

If you have `psql` installed, you can use it to connect just like you would
to any Postgres running on localhost.

If you don't, you can use `psql` without installing it by running it from
within Docker using this script:

```sh
./psql.sh
```

To run the frontend build process:

```sh
# if you don't have gulp installed globally
export PATH="$PWD/node_modules/.bin:$PATH"

gulp watch
```

To run the server:

```sh
node server
```

### Files

* See the `config` object in `gulpfile.js` for locations of the frontend files.
* Database contents is in `db`. You can shut the database down and move that
    directory aside to start fresh.
* S3 contents (files) are in `s3-minio`. You can shut minio (docker-compose)
    down and move that directory aside to start fresh.

### Database

* The database is Postgres.
* The API uses [`hapi-node-postgres`](https://github.com/jedireza/hapi-node-postgres)
    (which uses [`node-postgres`](https://github.com/brianc/node-postgres)
    ([`pg`](https://www.npmjs.com/package/pg))) directly.
* No ORM is in use. This probably makes development slower, but gives us
    less to learn and eliminates the likelihood of losing time to ORM
    limitations later.
* Eventually, we'll want a database migrations tool. There's a Trello card
    tagged technical debt for adopting one.

> If you ever include any variable in the first argument to `request.pg.query`,
> also include a half page comment on why the right way wouldn't work that time.
