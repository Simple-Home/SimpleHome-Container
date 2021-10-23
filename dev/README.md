npx cypress open 

docker-compose -f docker-compose_dev.yml build
docker-compose -f docker-compose_dev.yml up

npx cypress run