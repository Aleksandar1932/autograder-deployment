# About

AutoGrader is human-in-the-loop AI tool for grading text based answers from exams on Macedonian language. It was developed as final course project for the courses Web Programming and Introduction to Data Science at the Faculty of Computer Science and Engineering, Skopje, North Macedonia.

Authors:

- Aleksandar Ivanovski (AI + Web)
- Nikola Kanevche (AI + Web)
- Vangel Hristov (Web)

# Project Layout

The project is structured in tree modules, i.e. front-end, ml-model and back-end necessary for data management and process modeling.

The front-end delegates the whole process and serves as main entry for the users using the system for grading answers mainly teaching staff.

The ml-model is available as REST API and as python library. The API is consumed by front-end, and the python library can be imported as standard python library and provide the same functionalities. Main purpose for this separation was to provide entry point for potential users, i.e. REST API, and to enable other researchers to leverage our model for further research.

The back-end is responsible for managing data necessary for the whole process, i.e. exams, courses, users, grades, questions, answers and to provide stateless user authentication and authorization with JWT tokens.

# Code Repositories

The code base consists of tree repositories available on GitHub, i.e.

- [autograder-ml-model](https://github.com/Aleksandar1932/autograder-ml-model)
- [autograder-frontend](https://github.com/Aleksandar1932/autograder-frontend)
- [autograder-spring-backend](https://github.com/Aleksandar1932/autograder-spring-backend)

Above mentioned repositories are private and authors should be contacted for granting access.

# Tech Stack

| Module                    | Main Language | Framework   |
| ------------------------- | ------------- | ----------- |
| autograder-ml-model       | Python        | Flask       |
| autograder-spring-backend | Java          | Spring Boot |
| autograder-frontend       | javascript    | next.js     |

# Docker Images Setup

`autograder-ml-model` and `autograder-frontend` are dockerized and can be ran in containers, while `autograder-spring-backend` is already deployed on Heroku and available as REST API through public URI, available [here](https://autograder-spring-backend.herokuapp.com/).

First two docker images should be build for dockerized modules.

## autograder-ml-model

### Storage Volume

1. Open arbitrary directory containing FastText MK model, see [this](https://dl.fbaipublicfiles.com/fasttext/vectors-crawl/cc.mk.300.bin.gz).

2. `docker volume create autograder-volume`

3. `docker run -d --rm --name dummy -v autograder-volume:/root alpine tail -f /dev/null`

4. `docker cp <cc.mk.300.bin location on local FS> dummy:/root/cc.mk.300.bin`

5. `docker rm dummy`

### Image

1. Open project root

2. `docker build -t autograder-ml-model .`

3. `docker run -d --name "autograder-ml-model-container" -v autograder-volume:/root -p 8081:5000 autograder-ml-model` (if docker-compose is ommited)

## autograder-frontend

1. Open project root
2. `docker build -t autograder-frontend` .
3. `docker run --name "autograder-frontend-container" -p 8082:8086 autograder-frontend` (if docker-compose is ommited)

# Environment Variables

## autograder-ml-model

    BACKEND_URL=https://autograder-spring-backend.herokuapp.com/
    ENDPOINT_PREFIX=/api/model
    USERNAME=ml-model
    PASSWORD=ml-model

After images are build, verification could be done with running `docker images`, and `autograder-ml-model`, `autograder-frontend` should be listed. Docker compose could be used to run the images in two containers, as follows:

1. Open this project's root
2. `docker-compose up`
3. ml-model should be available on port `8081` and frotnend on port `8082`
