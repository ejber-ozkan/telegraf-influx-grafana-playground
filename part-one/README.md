# Part one

Part one runs a single container application without any TIG integration, the application should log to standard out, which is viewable in the docker Console UI.

## Cheat sheet

To build part one

```bash
docker-compose build --no-cache
```

To run part one

```bash
docker-compose up -d
```

To stop part one

```bash
docker-compose down 
```

When running the common-api example should be reachable on a browser

<http://localhost:8080/status>

(returns a JSON mimetype)

<http://localhost:8080/hello>

(returns a plain text mimetype)

## Additional commands

You might have to create a volume as external for the first time.

```bash
docker volume create --name=grafana-volume
docker volume create --name=influxdb-volume
docker volume create --name=loki-volume
```

To list the volumes created or available.

```bash
docker volume ls
```

When running to list the networks.

```bash
docker network ls 
```

You may wish to produce multiple events to view from the example application endpoints, this can be done through curl commands to the sample application endpoints.

```bash
for i in 0 1 2 3 4 5 6 7 8 9; do curl localhost:8080/status ; done
for i in 0 1 2 3 4 5 6 7 8 9; do curl localhost:8080/hello ; done
```
