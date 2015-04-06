# MongoDB Arbiter Docker Image

This image is mainly based on the offical [MongoDB Docker image](https://registry.hub.docker.com/_/mongo/) with only a few changes required to run an arbiter in the replica set.

However it doesn't directly use the offical image as the base image, it uses the original Dockerfile and docker-entrypoint.sh as base for changes instead.

Changes:
* Change all "/data/db" to "/data/arb" in Dockerfile and docker-entrypoint.sh
* Change the port from 27017 to 30000

## Usage

### Create a data volume container

```
docker create --name mongo-arbiter-data -v /data/arb busybox
```

### Launch a MongoDB arbiter

(Following command assumes that you use a replica set called "rs0".)

```
docker run --name mongo-arbiter --volumes-from=mongo-arbiter-data -p 30000:30000 -d killercentury/mongo-arbiter mongod --port 30000 --dbpath /data/arb --replSet "rs0" --smallfiles --nojournal --quiet
```

The "--quiet" parameter above mainly helps you to reduce the amount of logs being generated, otherwise they will grow dramatically over a few days and the instance running this container will run out of disk space at the end.

For other parameters, please refer to the offical MongoDB documentation.
