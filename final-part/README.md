# Final Part

The final part runs six (6!) containers, the application from part one with Telegraf, and the other two as Grafana and influxDB, It also introduces Loki the log aggregator. The Jaeger container is added for Tracing. Finally a second API example this time written in python that should trace out.

## Cheat sheet

To build final part

```bash
docker-compose build --no-cache
```

To run final part

```bash
docker-compose up -d
```

To stop final part

```bash
docker-compose down 
```

Grafana UI should be accessible on local port 3000

<http://localhost:3000/>

Default Default Grafana Docker image UI credentials

 `username:admin password:admin`

<http://localhost:3000/datasources/edit/1/>

InfluxDB will be available to access on port 8086

To add InfluxDB as a data source go to

<http://localhost:3000/datasources/edit/1/>

in the UI setup wizard form for InfluxDB in Grafana, enter below and hit Test and apply.

URL: `http://influxdb:8086`
database: `telegraf`
username: `admin`
password: `supersecretpassword`

These are all pre launch settings and can be changed in the [docker-compose.yml](docker-compose.yml) file.

Loki runs on port 3100

To add Loki as a data source
<http://localhost:3000/datasources/edit/1/>

Loki by default doesn't need credentials entering the URL should be enough.

URL: `http://loki:3100`

To add Jaeger as a data source

<http://localhost:3000/datasources/edit/1/>

Jaeger runs on a number of ports including a UI port of 16686

URL: `http://jaeger:16686`

Both Jaeger and Loki can be explored through the Grafana Explorer menu item.

When running the common-api example should be reachable on a browser

<http://localhost:8080/status>

(returns a JSON mimetype)

<http://localhost:8080/hello>

(returns a plain text mimetype)

When running the Python flask-example should be reachable on a browser

<http://localhost:5000>

## Additional commands

You might have to add the loki `plugin` for Docker

```bash
docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
```

To list plugins available

```bash
docker plugin ls
```

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
for i in 0 1 2 3 4 5 6 7 8 9; do curl localhost:5000 ; done
for i in 0 1 2 3 4 5 6 7 8 9; do curl localhost:5000 ; done
```

You may wish to login directly to the influxdb container to test and run some commands.

Login to InfluxDB container

```bash
docker exec -it  -u 0 influxdb sh -il
```

Within the container shell

Show databases

```bash
influx -execute 'SHOW DATABASES'
```

Run a select query, example return all sensors metrics from the Telgraf Database.

```bash
influx -execute 'Select * from "sensors" limit 3' -database="telegraf" -precision=rfc3339
```

Go into the InfluxDB command line interface, and then run some example queries directly.

```sql
influx -precision rfc3339
$
SHOW DATABASES
USE telegraf
SHOW SERIES
SHOW measurements ON telegraf 
Select * from cpu limit 10
```

Curl a query through the InfluxDB API.

```bash
curl -G "http://localhost:8086/query?db=telegraf&pretty=true" --data-urlencode "q=SHOW MEASUREMENTS"
```
