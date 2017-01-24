## Assignment 3 - Django DRF Continuous Integratation - Part 1

### Prerequsites
- Student has Github account
- Student has Docker installed

### Concepts Introduced
- Docker Containers
- Review DRF

### Step 1 - Create Local Project Directory
create a directory called fooProject and cd into it:
```
mkdir fooProject && cd fooProject
```

### Step 2 - Create Docker Environment

In the base project directory using your favorite editor create a file called: **Dockerfile** with the following contents

```yaml

FROM python:3.4
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```

Create a the docker compose file (**docker-compose.yml**) with the following contents that describes the web and db containers and linkages:

```
db:
  image: postgres
web:
  build: .
  command: gunicorn fooProject.wsgi:application -b :8000
  volumes:
    - .:/code
  ports:
    - "8000:8000"
  links:
    - db
```

**Notes:**

* We give our web container access to the db container with the `link` command
* Our app is exposed on port 8000. 
* When we run `docker-compose up`, the `command` with be executed
* Standard stack. Uses Postgres as the database.
* We use gunicorn server. This is a production ready server so that we can run the same `docker-compose` in production. 

Create the requirements **requirements.txt** file that describes our dependencies:

```
django==1.9.2 
djangorestframework==3.3.2
django-filter
django-rest-swagger
psycopg2
requests

sniffer
django-extensions
coverage
responses
mock

gunicorn

```
* Specify the latest version of django and django rest framework
* The second block is a list of tools for testing and quality control
* gunicorn is the server

### Create a blank Django Project

In a docker terminal: 

```
docker-compose run web django-admin.py startproject todoapi .
```

This might take a while to run the first time as it downloads all the base images and does any legwork defined in `Dockerfile`.

**Notes:**

* This will run `docker-compose up` and setup our docker environment as specified in our `docker-compose.yml` file. 
* All commands should be run with `docker-compose`. This will run commands within our container. You should never run a command directly on your box.

You should now see a new folder called `todoapi` in the current directory. 

At this stage, your top level directory should look something like this:

```
Dockerfile
docker-compose.yml
manage.py
requirements.txt
[todoapi]
```

### Playing with your application: 

**To run the app**

```
docker-compose up
```

**To run the tests**

```
docker-compose run web python manage.py test
```

**To auto-run the tests**

Create a file called scent.py:

```
from subprocess import call
from sniffer.api import runnable

@runnable
def execute_tests(*args):
    fn = [ 'python', 'manage.py', 'test' ]
    fn += args[1:]
    return call(fn) == 0
```

Then you can run: 

```
docker-compose run web sniffer
```

* This will automatically run the tests each time you make changes to the code

**Generate a test coverage report**

**Generate coverage:**

```
docker-compose run web coverage run --source='.'  manage.py test
```

**Generate an html report**

```
docker-compose run web coverage html --directory=reports
```

You will then find your coverage report in `reports/index.html`
