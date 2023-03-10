## Run command
### Get everything working with:
```
docker-compose up
```
### See the result at:
```
http://localhost:8080/
```
***
## Build steps
### Create the network
```
docker network create backend-net --driver bridge
```
### Build the images
```
docker build -t stephano1234/mysql-db mysql-example/.
```
```
docker build -t stephano1234/node-backend node-example/.
```
```
docker build -t stephano1234/nginx-proxy nginx-example/.
```
### Run the containers in proper order
#### First container
```
docker run --rm --name mysql-db --network backend-net --mount type=bind,source=/home/main/dockerfiles/mysql-example/mysql/,target=/var/lib/mysql -d stephano1234/mysql-db
```
#### Second container
```
docker run --rm --name node-backend --network backend-net --mount type=bind,source=/home/main/dockerfiles/node-example,target=/usr/src/app -d stephano1234/node-backend node index.js
```
#### Third container
```
docker run --rm --name nginx-proxy --network backend-net -p 8080:80 -d stephano1234/nginx-proxy
```