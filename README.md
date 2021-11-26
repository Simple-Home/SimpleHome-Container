# SimpleHome-Container

Development Docker Container containing for testing and deploying

## Build and run container

```shell
docker-compose build
docker-compose up -d
```

## Go into container

```shell
docker-compose exec -u application app /bin/bash
```

# Docker Dev

## Build and run container

```shell
docker-compose -f docker-compose.yml build
docker-compose -f docker-compose.yml up
```

# Run cypress test

```shell
docker-compose exec -u application app /bin/bash
npx cypress run
```
