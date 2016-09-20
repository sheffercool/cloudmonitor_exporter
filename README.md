# Cloudmonitor exporter
Prometheus exporter for Akamai Cloudmonitor statistics.
Retrieves json data from akamai cloudmonitor [Here](https://control.akamai.com/dl/customers/ALTA/Cloud-Monitor-Implementation.pdf) on `collector.endpoint` and provides metrics on `metrics.endpoint`

## Get it
The latest version can be found under [Releases](https://github.com/ExpressenAB/cloudmonitor_exporter/releases).

## Usage
Example: 
```
./cloudmonitor_exporter
```

## Flags
Flag | Description | Default
-----|-------------|---------
-exporter.address | Exporter bind address:port | :9143
-exporter.namespace | The namespace used in prometheus labels | cloudmonitor
-metrics.endpoint | Metrics endpoint | /metrics
-collector.endpoint | Collector endpoint | /collector
-collector.accesslog | File to store accesslogs to | "" off

## Description
Akamai cloud monitor will aggregate client request/responses as json data and send them to specified endpoint.
Detailed information can be found [Here](https://control.akamai.com/dl/customers/ALTA/Cloud-Monitor-Implementation.pdf)

This exporter will gather this data and expose an `/metrics` endpoint to prometheus.

## Basic setup with docker-compose
To setup an small test environment using docker-compose:
* Make sure docker-compose is installed ([instructions](https://docs.docker.com/compose/install/))
* Clone this repository

`git clone git@github.com:ExpressenAB/cloudmonitor_exporter.git`
* Create self-signed certificate by running setup.sh
```
> cd cloudmonitor_exporter/setup
> ./setup.sh
This will generate a self-signed certificate to use with cloudmonitor_exporter
Enter companyname for certificate:
......
```
* Start containers by running `docker-compose up`

Now we have 4 docker containers running with:
* cloudmonitor_exporter listening on :9143
* haproxy listening on 443 with self-signed certificate for ssl termination
* prometheus scraping localhost:9143
* grafana using prometheus datasource from localhost:9090

To be able to retrieve cloudmonitor data to the running exporter, you need to to create an cloudmonitor property, that other properties will send loglines to.

For example:

![alt text](docs/akamai_config.png "Akamai config")

Then it's enough to add the following behavior to your properties.

![alt text](docs/akamai_behavior.png "Akamai behavior")

When properties are active and data is retrieved we will be able to query prometheus.

![alt text](docs/prometheus.png "Prometheus")

An example of metrics used on grafana dashboard
![alt text](docs/grafana.png "Prometheus")






