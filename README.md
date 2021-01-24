# Casting Agncy

## Project Motivation 

Casting agncy final project for udacity aims to collect data of actors and movies and to de any modification on them. the project has three users: 1.Casting Assistant 2. Casting Director 3. Casting Director each of them has a specefic role and permissions. 

## Getting Started

### URLs
Locally: http://127.0.0.1:5000/
Heroku: https://casting---agency.herokuapp.com

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment
Create Virtual Env by running:

```bash
virtualenv --python=python3.7 venv

```
#### Activate Virtual Enviornment

```bash
source  venv/bin/activate

```
#### PIP Dependencies

install dependencies by running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) and [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) 

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

## Running the server

first you have to export the flask app, run:

```bash
export FLASK_APP=app.py;
```

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

To run the test cases, execute:

```bash
python test_app.py
```
#### Setup Authentication in Auth0: 

1. Create a new Auth0 Account
2. Select a unique tenant domain
3. Create a new, single page web application
4. Create a new API
4. in API Settings:
    - Enable RBAC
    - Enable Add Permissions in the Access Token
5. Create new API permissions:
    - get:actors
    - get:movies
    - delete:actors
    - delete:movies
    - post:actors
    - post:movies
    - patch:actors
    - patch:movies

6. Create new roles for:
    - Casting Assistant:
       Can view actors and movies
    - Casting Director:
      - All permissions a Casting Assistant has and…
      - Add or delete an actor
      - Modify actors or movies
    - Executive Producer:
       Can perform all actions



## Endpoints
```
# Movies

GET '/movies'
POST '/movies'
PATCH '/movies/<id>'
DELETE '/movies/<id>'

GET '/movies'
- Fetches all movies on the platform
- Request Arguments: None
- Allowed users: Executive Producer, Casting Assistant and casting Director
- Required permission (get:movies)
- Response
{
  "movies": [
    {
      "id": 88,
      "release_date": "Sun, 23 Jun 2018 00:00:00 GMT",
      "title": "Instant Family"
    },
    {
      "id": 89,
      "release_date": "Mon, 28 Jun 2020 00:00:00 GMT",
      "title": "soul"
    }
  ],
  "success": True
}

POST '/movies'
- Creates a new movie with the provided parameters
- Request Arguments: None
- Allowed users: Executive Producer
- Required permission (add:movies)
- Request Body: {
	"title": "Slap of the Century"
	"release_date": "2/26/1996"
}

- Response
{
  "movie": {
    "id": 4,
    "release_date": "Mon, 26 Feb 1996 00:00:00 GMT",
    "title": "Slap of the Century"
  },
  "success": true
}


PATCH '/movies/<id>'
- Updates a specific movie with the provided parameters
- Request Arguments: movie_id (The ID of the movie to update)
- Allowed users: Executive Producer, Casting Director
- Required permission (edit:movies)
- Request Body: {
	"title": "Silicon Valley"
}

- Response
{
  "movie": {
    "id": 4,
    "release_date": "Thu, 26 Feb 2004 00:00:00 GMT",
    "title": "Silicon Valley"
  },
  "success": true
}

DELETE '/movies/<id>'
- Deletes a specific movie
- Request Arguments: movie_id (The ID of the movie to delete)
- Allowed users: Executive Producer
- Required permission (delete:movie)
- Response
{
  "delete": id,
  "success": true
}



# Actors

GET '/actors'
POST '/actors'
PATCH '/actors/<id>'
DELETE '/actors/<id>'

GET '/actors'
- Fetches all actors on the platform
- Request Arguments: None
- Allowed users: Executive Producer, Casting Assistant and casting Director
- Required permission (get:actors)
- Response
{
  "actors": [
    {
      "age": 53,
      "gender": "male",
      "id": 3,
      "name": "Jamie Foxx"
    },
    {
      "age": 40,
      "gender": "female",
      "id": 1,
      "name": "Rose Byrne"
    }
  ],
  "success": true
}

POST '/actors'
- Creates a new actor with the provided parameters
- Request Arguments: None
- Allowed users: Executive Producer and casting Director
- Required permission (add:actors)
- Request Body: {
	"name": "Mark Wahlberg",
	"age": 48,
	"gender": "male"
}

- Response
{
  "actor": {
    "age": 48,
    "gender": "male",
    "id": 87,
    "name": "Mark Wahlberg"
  },
  "success": true
}


PATCH '/actors/<id>'
- Updates a specific actor with the provided parameters
- Request Arguments: actor_id (The ID of the actor to update)
- Allowed users: Executive Producer and Casting Director
- Required permission (edit:actor)
- Request Body: {
	"name": "name1",
}

- Response
{
  "actor": {
    "age": 48,
    "gender": "male",
    "id": 87,
    "name": "name1"
  },
  "success": true
}

DELETE '/actors/<id>'
- Deletes a specific actor
- Request Arguments: actor_id (The ID of the actor to delete)
- Required permission (delete:actors)
- Response
{
  "delete": id,
  "success": true
}

Errors 
For errors, the server returns a json object with a description of the type of error. Find the description below:

400 (Bad Request)
  {
    "success": False, 
    "error": 400,
    "message": "bad request, please check your input"
  }

401 (Unauthorised)
  {
    "success": False, 
    "error": 401,
    "message": "authorisation error"
  }

403 (Forbiddden request)
  {
    "success": False, 
    "error": 403,
    "message": "You are not allowed to access this resource"
  }

404 (Resource Not Found)
  {
    "success": False, 
    "error": 404,
    "message": "resource not found"
  }

405 (Method not allowed)
  {
    "success": False, 
    "error": 405,
    "message": "Method not allowed"
  }

422 (Unprocessable entity)
  {
    "success": False, 
    "error": 422,
    "message": "unprocessable"
  }

500 (Internal server error)
  {
    "success": False, 
    "error": 500,
    "message": "Server error
  }
```