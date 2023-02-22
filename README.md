# toolhunt

# Description

Toolhunt is a web application for editing Toolhub records in a fun and easy way.
It allows users to view and add missing fields for tools in Toolhub.

This repository contains the project backend.
The frontend can be found in the [toolhunt-ui repository](https://github.com/wikimedia/toolhunt-ui).

This is an [Outreachy](https://www.outreachy.org/) Internship project.

## Issue Tracker

This project uses [Phabricator](https://phabricator.wikimedia.org/project/board/6283/) to track issues.

## Documentation

API documentation is automatically generated by Swagger and can be accessed (while the app is running) at `localhost:8082/api/documentation`

## Setup/Installation Requirements

Note that, as of the latest build, access to the API documentation and backend functionality happens through the frontend component of this app: [toolhunt-ui repository](https://github.com/wikimedia/toolhunt-ui)

In other words, to do much of anything with this half of the project, you **must** have both front- and backend components running in Docker containers.

- Clone this repo to your machine with the command `git clone https://github.com/wikimedia/toolhunt.git`

- Clone the frontend ui repo with `git clone https://github.com/wikimedia/toolhunt.git`

In order to authenticate successfully, you must register your local version of the app with the [Toolhub Demo Server](https://toolhub-demo.wmcloud.org/).

- To register, go to [Developer Settings](https://toolhub-demo.wmcloud.org/developer-settings?tab=oauth-register) and complete the form.

- Set `http://localhost:8082/api/authorize` as the Authorization Callback URL.

- Create an `.env` file in the root directory of your toolhunt repo and add `TOOLHUB_CLIENT_ID` and `TOOLHUB_CLIENT_SECRET` (set their values to be whatever you received from the demo server registration)

When building the Docker containers, the backend **must** go first.

From the toolhunt directory:

- `docker-compose up --build --detach`

From the toolhunt-ui directory:

- `docker-compose up --build --detach`

Then:

- Open a browser window to localhost:8082

### To initialize the Database

- From the command line, `docker-compose exec flask-web flask db upgrade`

### To access the Database

- From the command line, `docker exec -it mariadb mariadb --user user -p mydatabase` (password: mypassword)

### Adding bulk data to the Database

Whether you're working with the mock data or "real" data, the contents of the `field` table will remain the same.

- From the command line, `docker-compose exec flask-web python manage.py insert_fields`

#### To insert mock data into the Database

The mock data set contains three tools and a set of completed tasks. When run, the function `populate_db_test` will put the tools through the insertion process and auto-generate tasks, as it would with "real" data.

The results will appear on the command line. This command may be run multiple times; observe the command line messages to see what changes when a tool and/or task is already present in the DB.

The set of completed tasks will allow us to test that the high scores, user contributions, and latest contributions are returning correctly.

- From the command line, `docker-compose exec flask-web python manage.py populate_db_test`

#### To insert a full data set into the Database

- From the command line, `docker-compose exec flask-web python manage.py populate_db_initial`

While in development mode, this fetch request draws data from the Toolhub Test Server. It can be run prior to or following `populate_db_test`.

## Technologies to be Used

- Python
- Flask
- Redis
- Docker
- MariaDB
