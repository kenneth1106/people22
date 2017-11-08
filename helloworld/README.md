# README

## Init project
``` 
$ docker run --rm -v $(pwd):/usr/srv/app -w /usr/srv/app rails rails new . --skip-bundle
$ docker run --rm -v $(pwd):/usr/src/app -w /usr/src/app ruby:2.3.1 bundle lock

# option
$ docker run --rm -v $(pwd):/usr/src/app -w /usr/src/app ruby:2.3.1 bundle lock --update
```

### Dockerfile
```
FROM ruby:2.3.3
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
ADD Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
ADD . /myapp
CMD bundle exec rails s -p 3000 -b 0.0.0.0
```

### docker-compose.yml
```
version: '3'
services:
  db:
    image: postgres
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

### Rails controller
```
$ docker exec helloworld_web_1 rails generate controller home index
```

### Build
- docker build
- docker-compose build

## Q&A
- [Difference between links and depends_on in docker_compose.yml](https://stackoverflow.com/questions/35832095/difference-between-links-and-depends-on-in-docker-compose-yml)
