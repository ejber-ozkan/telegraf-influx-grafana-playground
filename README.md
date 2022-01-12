# TIG playground

This repo has several examples of building a docker compose services that interact together to form a Telegraf, InfluxDB and Grafana "TIG" stack.

The group of services uses two simple API example containers (built in golang and python flask) to output metrics,tracing and logging that are built from another docker file (in another repo), however provided your service can also be containerised these could easily be replaced or pointed to a different repository of your choice.

Each part contains a README to follow bit like a part of a course, but if you'd like to get up and running quickly skip to the [final part](final-part/README.md)

## Prerequisites

Latest Docker suite on your desktop.
Some knowledge of docker, docker compose.
Some Linux knowledge would be useful.
Some knowledge of Grafana UI ( The G in TIG stack).

Useful to know some golang or python for the example containers.

This hasn't been tested on a Windows based Platform.