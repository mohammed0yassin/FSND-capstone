
# FSND Capstone Project: Casting Agency API

## Full Stack Casting Agency

This is the final project of `Udacity Full Stack Web Developer Nanodegree` 

The Project is the backend of a Casting Agency Company that hires actors and produce movies.

There are different roles with different permissions for people who work in the agency.


The Project Contains:
- Database modeling with PostgreSQL & SQLAlchemy
- APIs to performance CRUD Operations on database with Flask
- Automated testing with Unittest
- Automatad testing with Postman
- Authorization & Role based authentication control with Auth0 
- Web server deployment on Heroku
  

## Getting Started

  

### Installing Dependencies

  

#### Python 3.8.2

  

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

  

#### Virtual Enviornment

  

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

  

#### PIP Dependencies

  

Once you have your virtual environment setup and running, install dependencies by running:

  

```bash

pip install -r requirements.txt

```

  

## Running the server

  

### Locally

  

Setup the data base

  

```shell

createdb agency

```

Populate the database with data from `agency.psql`

```shell

PGPASSWORD=mypassword psql -U postgres -d agency -1 -f agency.psql

```

  

From within this directory first ensure you are working using your created virtual environment.

  

Each time you open a new terminal session, run:

  

```bash

source setup.sh

```

Which will have all the required environment variables to run the app locally

Some of `setup.sh` variables:

```bash

export FLASK_APP=app.py

export FLASK_ENV=development

```

  
  

To run the server, execute:

  

```bash

flask run

```

The app will be running on:

```txt

http://127.0.0.1:5000/

```

  
  

### The app is also running on the Production Server

  

```txt

https://cs-agency.herokuapp.com/

```

## Running Tests (Unittests)

Setup the test database

```shell

createdb agency_test

```

Populate the database with data from `agency.psql`

```shell

PGPASSWORD=mypassword psql -U postgres -d agency_test -1 -f agency.psql

```

  

Each time you open a new terminal session, run:

  

```bash

source setup.sh

```

Which will have all the required environment variables to run the app locally

  

Currently for review purposes the following tokens are also set via environment variables, and provided in the `setup.sh` configuration:

-  `exec_producer_token`

-  `casting_director_token`

-  `casting_assistant_token`

For ease you could run `database_setup.sh` script which will drop, create then populate the database each time before you run the test.

```shell

./database_setup.sh

```
`database_setup.sh` has the following commands

```script
#!/bin/bash

PGPASSWORD=mypassword dropdb -U postgres agency_test
PGPASSWORD=mypassword createdb -U postgres agency_test
PGPASSWORD=mypassword psql -U postgres -d agency_test -1 -f agency.psql 1>&1 | grep nothing # grep to mute output
```



Run tests against local testing database

```shell

python test_app.py

```
## Running Tests using Postman

### Test locally:
>Import the postman collection `Casting-Agency.postman_collection.json`
>
>Restore backup to the `agency_test` database
>
>For ease you could run `database_setup.sh` script which will drop, create then >populate the database each time before you run the collection.
>
>```shell
>
>./database_setup.sh
>
>```
>`database_setup.sh` has the following commands
>
>```script
>#!/bin/bash
>
>PGPASSWORD=mypassword dropdb -U postgres agency_test
>PGPASSWORD=mypassword createdb -U postgres agency_test
>PGPASSWORD=mypassword psql -U postgres -d agency_test -1 -f agency.psql 1>&1 | grep nothing # grep to mute output
>```



### Test on the production server:

>Import the postman collection `Casting-Agency-Heroku.postman_collection.json`
>
>Restore the database on the Heroku server using
>
>```shell
>heroku pg:backups:restore b002 DATABASE_URL --app cs-agency --confirm cs-agency
>```
>**Note** The backup requires Heroku login credentials 



## All Available Endpoints
| Endpoints | Details | Required Permission |
|--|--|--|
| GET '/shows' | A public endpoint for anyone to view the shows of the agency | None |
| GET '/actors'| An endpoint to view the actors of the agency| get:actors |
| GET '/movies'| An endpoint to view the movies of the agency| get:movies |
| POST '/actors' | An endpoint that adds a new actor | post:actors |
| POST '/movies' | An endpoint that adds a new movie | post:movies |
| POST '/shows' | An end point to add a new show | post:shows |
| PATCH '/actors/`<id>`' | An endpoint to edit an actor| patch:actors |
| PATCH '/movies/`<id>`' | An endpoint to edit a movie| patch:movies |
| DELETE '/actors/`<id>`' | An endpoint to delete an actor | delete:actors |
| DELETE '/movies/`<id>`' | An endpoint to delete an movie | delete:movies |
|DELETE '/shows/`<movie_id>`'| An endpoint to delete a show using its movie_id | delete:shows |
|--|--|--|

## Roles and their permissions

| Role | Permissions |
|--|--|
| Executive Producer | `post:shows` `post:movies` `post:actors` `patch:movies` `patch:actors` `get:movies` `get:actors` `delete:shows` `delete:movies` `delete:actors`|
| Casting Director | `post:actors` `patch:movies` `patch:actors` `get:movies` `get:actors` `delete:actors`|
| Casting Assistant | `get:movies` `get:actors`|

## Endpoints

#### GET '/shows' 
> - Fetchs a list of shows
> - Returns `JSON` containing all available shows
>```json
>>   {
>    "shows": [
>        {
>            "Actors": [
>                {
>                    "age": 20,
>                    "gender": "Male",
>                    "id": 1,
>                    "name": "Mohammed"
>                },
>                {
>                    "age": 20,
>                    "gender": "Female",
>                    "id": 2,
>                    "name": "Nadine"
>                }
>            ],
>            "movie_id": 1,
>            "movie_title": "Rise of the APIs",
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT"
>        },
>        {
>            "Actors": [
>                {
>                    "age": 26,
>                    "gender": "Male",
>                    "id": 3,
>                    "name": "Jack"
>                },
>                {
>                    "age": 23,
>                    "gender": "Male",
>                    "id": 4,
>                    "name": "Will"
>                },
>                {
>                    "age": 27,
>                    "gender": "Female",
>                    "id": 5,
>                    "name": "Scarlet"
>                }
>            ],
>            "movie_id": 2,
>            "movie_title": "The New Upcoming Movie",
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT"
>        }
>    ],
>    "success": true
>}
>```

### GET '/actors'
>- Fetchs a list of all actors with their details
>- Returns `JSON` containing actors details
>```json
>{
>    "actors": [
>        {
>            "age": 20,
>            "gender": "Male",
>            "id": 1,
>            "name": "Mohammed"
>        },
>        {
>            "age": 20,
>            "gender": "Female",
>            "id": 2,
>            "name": "Nadine"
>        },
>        {
>            "age": 26,
>            "gender": "Male",
>            "id": 3,
>            "name": "Jack"
>        },
>        {
>            "age": 23,
>            "gender": "Male",
>            "id": 4,
>            "name": "Will"
>        },
>        {
>            "age": 27,
>            "gender": "Female",
>            "id": 5,
>            "name": "Scarlet"
>        }
>    ],
>    "success": true
>}
>```

### GET '/movies'
>- Fetches a list of movies
>- Returns a list of movies and their details
>```json
>{
>    "movies": [
>        {
>            "id": 1,
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT",
>            "title": "Rise of the APIs"
>        },
>        {
>            "id": 2,
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT",
>            "title": "The New Upcoming Movie"
>        }
>    ],
>    "success": true
>}
>```


### POST '/actors'
>- Adds a new actor
>- Returns a list of actors details including the newly added actor
>```json
>{
>    "actors": [
>        {
>            "age": 20,
>            "gender": "Male",
>            "id": 1,
>            "name": "Mohammed"
>        },
>        {
>            "age": 20,
>            "gender": "Female",
>            "id": 2,
>            "name": "Nadine"
>        },
>        {
>            "age": 26,
>            "gender": "Male",
>            "id": 3,
>            "name": "Jack"
>        },
>        {
>            "age": 23,
>            "gender": "Male",
>            "id": 4,
>            "name": "Will"
>        },
>        {
>            "age": 27,
>            "gender": "Female",
>            "id": 5,
>            "name": "Scarlet"
>        },
>        {
>            "age": 22,
>            "gender": "Male",
>            "id": 6,
>            "name": "POST Actor"
>        }
>    ],
>    "success": true
>}
>``` 

### POST '/movies'
>- Adds a new movie
>- Returns a list of movies including the newly added movie
>```json
>    {
>    "movies": [
>        {
>            "id": 1,
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT",
>            "title": "Rise of the APIs"
>        },
>        {
>            "id": 2,
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT",
>            "title": "The New Upcoming Movie"
>        },
>        {
>            "id": 3,
>            "release_date": "Wed, 15 Jan 2025 15:30:00 GMT",
>            "title": "POST Movie"
>        }
>    ],
>    "success": true
>}
>```

### POST '/shows
>- Adds a new show
>- Returns a list of shows including the newly added show
>```json
>{
>    "shows": [
>        {
>            "Actors": [
>                {
>                    "age": 20,
>                    "gender": "Male",
>                    "id": 1,
>                    "name": "Mohammed"
>                },
>                {
>                    "age": 20,
>                    "gender": "Female",
>                    "id": 2,
>                    "name": "Nadine"
>                }
>            ],
>            "movie_id": 1,
>            "movie_title": "Rise of the APIs",
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT"
>        },
>        {
>            "Actors": [
>                {
>                    "age": 26,
>                    "gender": "Male",
>                    "id": 3,
>                    "name": "Jack"
>                },
>                {
>                    "age": 23,
>                    "gender": "Male",
>                    "id": 4,
>                    "name": "Will"
>                },
>                {
>                    "age": 27,
>                    "gender": "Female",
>                    "id": 5,
>                    "name": "Scarlet"
>                }
>            ],
>            "movie_id": 2,
>            "movie_title": "The New Upcoming Movie",
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT"
>        },
>        {
>            "Actors": [
>                {
>                    "age": 26,
>                    "gender": "Male",
>                    "id": 3,
>                    "name": "Jack"
>                },
>                {
>                    "age": 22,
>                    "gender": "Male",
>                    "id": 6,
>                    "name": "POST Actor"
>                }
>            ],
>            "movie_id": 3,
>            "movie_title": "POST Movie",
>            "release_date": "Wed, 15 Jan 2025 15:30:00 GMT"
>        }
>    ],
>    "success": true
>}
>```

>### PATCH '/actors/`<id>`'
>- Edits desired attributes of an actor
>- Returns a list containg all actors including the edited actor
>```json
>{
>    "actors": [
>        {
>            "age": 20,
>            "gender": "Male",
>            "id": 1,
>            "name": "Mohammed"
>        },
>        {
>            "age": 20,
>            "gender": "Female",
>            "id": 2,
>            "name": "Nadine"
>        },
>        {
>            "age": 26,
>            "gender": "Male",
>            "id": 3,
>            "name": "Jack"
>        },
>        {
>            "age": 23,
>            "gender": "Male",
>            "id": 4,
>            "name": "Will"
>        },
>        {
>            "age": 27,
>            "gender": "Female",
>            "id": 5,
>            "name": "Scarlet"
>        },
>        {
>            "age": 22,
>            "gender": "Male",
>            "id": 7,
>            "name": "POST Actor"
>        },
>        {
>            "age": 30,
>            "gender": "Male",
>            "id": 6,
>            "name": "EDITED Actor"
>        }
>    ],
>    "edited": "6",
>    "success": true
>}
>```

### PATCH '/movies/`<id>`'
>- Adds a new movie
>- Args: the `id` of the movie
>- Returns a list of movies including the edited movie
>```json
>{
>    "edited": "3",
>    "movies": [
>        {
>            "id": 1,
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT",
>            "title": "Rise of the APIs"
>        },
>        {
>            "id": 2,
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT",
>            "title": "The New Upcoming Movie"
>        },
>        {
>            "id": 3,
>            "release_date": "Wed, 15 Jan 2025 15:30:00 GMT",
>            "title": "EDITED Title 2"
>        }
>    ],
>    "success": true
>}
>```

### DELETE '/actors/`<id>`'
>- Deletes an actor 
>- Args: The `id` of the actor
>- Returns the new list of actors after deleteing the selected actor
>```json
>{
>    "actors": [
>        {
>            "age": 20,
>            "gender": "Male",
>            "id": 1,
>            "name": "Mohammed"
>        },
>        {
>            "age": 20,
>            "gender": "Female",
>            "id": 2,
>            "name": "Nadine"
>        },
>        {
>            "age": 26,
>            "gender": "Male",
>            "id": 3,
>            "name": "Jack"
>        },
>        {
>            "age": 23,
>            "gender": "Male",
>            "id": 4,
>            "name": "Will"
>        },
>        {
>            "age": 27,
>            "gender": "Female",
>            "id": 5,
>            "name": "Scarlet"
>        }
>    ],
>    "deleted": "6",
>    "success": true
>}
>```

### DELETE '/movies/`<id>`'
>- Deletes a movie 
>- Args: The `id` of the movie
>- Returns the new list of movies after deleteing the selected movies
>```json
>{
>    "deleted": "3",
>    "movies": [
>        {
>            "id": 1,
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT",
>            "title": "Rise of the APIs"
>        },
>        {
>            "id": 2,
>            "release_date": "Mon, 21 Sep 2020 21:30:00 GMT",
>            "title": "The New Upcoming Movie"
>        }
>    ],
>    "success": true
>}
>```

### DELETE '/shows/`<movie_id>`'
>- Deletes a show using its `movie_id`
>- Args: The `movie_id` which is the id of the movie in the desired show to be >deleted
>- Returns the new list of the shows after deleting the desired show.
>```json
>{
>    "deleted": "2",
>    "shows": [
>        {
>            "Actors": [
>                {
>                    "age": 20,
>                    "gender": "Male",
>                    "id": 1,
>                    "name": "Mohammed"
>                },
>                {
>                    "age": 20,
>                    "gender": "Female",
>                    "id": 2,
>                    "name": "Nadine"
>                }
>            ],
>            "movie_id": 1,
>            "movie_title": "Rise of the APIs",
>            "release_date": "Sun, 11 Apr 2021 21:30:00 GMT"
>        }
>    ],
>    "success": true
>}
>```