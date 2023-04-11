# Description
This is a project, that can help you with building a monitiring system for a docker swarm orchestrator.
This monitoring system allows you to collect and watch statistics of cluster nodes, running services and containers.
It also provides for sending notifications about critical situations.

There are 4 types of alerts: CPU utilization over 80 percent, less than 20 percent free memory, less than 20 percent free disk space, container was stopped

In this case, notifications are sent to telegram, but you can use slack or another application. To use telegram alerts, you need to create a bot and a channel in telegram. Then, add the bot to the created channel. After that specify bot token and channel id in an alertmanager.yml file.
# Requirements
- docker engine
- initialized docker swarm cluster with at least one manager node
# Deployment
Build images
```
cd prometheus && docker build --tag dsm_prometheus:2.38.0 . && cd ..
cd alertmanager && docker build --tag dsm_prometheus/alertmanager:0.25.0 . && cd ..
```
Create volumes
```
docker volume create prometheus
docker volume create grafana
```
Create network
```
docker network create --driver overlay monitoring
```
Start the stack
```
docker stack deploy --with-registry-auth -c stack.yaml monitoring
```
After starting the services, grafana is available on port 3000 with the default login password admin. After changing the password, add prometheus as a datasource.

Add dashboards for services and container metrics\
https://grafana.com/grafana/dashboards/17023-docker/ \
And for nodes metrics\
https://grafana.com/grafana/dashboards/1860-node-exporter-full/
