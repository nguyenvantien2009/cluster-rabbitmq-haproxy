# RabbitMQ cluster with HAProxy and Docker Compose

Requirements:
1. Create `.env` from `example.env` file. Then change config account use for login.
2. 

Creates a 3 node RabbitMQ cluster with a HAProxy acting as a load balancer.

You need to build the HAProxy image first, just run:
```sh
$ docker build -t haproxy-server .
```

Now run the docker compose file:
```sh
$ docker-compose -f docker-compose.yml up
```

check if the containers are running:
```sh
$ docker ps
```

Create the cluster by running:
```sh
$ docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl stop_app"
$ docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl join_cluster rabbit@rabbitmq-node-1"
$ docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl start_app"

$ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl stop_app"
$ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl join_cluster rabbit@rabbitmq-node-1"
$ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl start_app"
```

Check the cluster status:
```sh
$ docker exec -ti rabbitmq-node-1 bash -c "rabbitmqctl cluster_status"
```

Access HAProxy statistics report at `http://localhost:1936/haproxy?stats` with the credential `haproxy:haproxy`, and the RabbitMQ console at `http://localhost:15672/` with the credential `admin:admin`.
