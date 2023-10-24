# AVM FRITZ!Box - SmartHome monitoring

Draw information about the installed AVM FRITZ!Box smarthome devices (powermeters, thermostat, ...) to grafana.

## Version

The latest configuration was validated with the following FRITZ!Box model, FRITZ!OS and docker images.

| FRITZ!OS | Box         |
|----------|----------------|
| 7.57    | FRITZ!Box 7590 |

| Software                     | Docker image                        | Version |
|------------------------------|-------------------------------------|---------|
| Prometheus                   | prom/prometheus                     | 2.31    |
| Grafana                      | grafana/grafana                     | 8.2     |
| FRITZ!Box smarthome exporter | jaymedh/fritzbox_smarthome_exporter | 0.4.3   |

## Motivation
Independent reading of the power consumption of AVM FRITZ devices, temperatures in rooms with a visualization (graphs) and the possibility of filtering / limiting the data.

## Architecture
`Grafana <- Prometheus <- FRITZ!Box SmartHome exporter`

## Installation

### Create network "monitoring"
`docker network create monitoring`

### Download Fritz!Box certificate
- Click "Internet" in the FRITZ! Box user interface.
- On the Internet menu, click Shares.
- Click the "FRITZ! Box Services" tab.
- Click on "Download Certificate" and save the file with the certificate on your computer. (Filename: boxcert.cer)


- Official documentation (german): https://avm.de/service/wissensdatenbank/dok/FRITZ-Box-7490/1523_FRITZ-Box-Zertifikat-herunterladen-und-am-Computer-importieren/
- SmartHome Exporter usage (english): https://github.com/jayme-github/fritzbox_smarthome_exporter

### Create .env file for docker-compose
```bash
# Fritz!Box SmartHome Exporter
FRITZ_USERNAME=
FRITZ_PASSWORD=
FRITZ_URL=https://192.168.178.1

# GRAFANA
GRAFANA_ADMIN_USERNAME=admin
GRAFANA_ADMIN_PASSWORD=grafanaAdmin! 
```
---

## Grafana

Url: `http://localhost:3000/` or `http://YOUR_SERVER_IPv4:3000/`

### Data sources
- Name: `Prometheus - FRITZ!Box`
- Url: `http://mon_prometheus:9090`
- Access: `Server`
- Skip TLS verify: `true`
- Scrape interval: `15s`

### Dashboard
Import the Grafana dashboard file `grafana-dashboard-template.json` and set the data source to the previous defined data source `Prometheus - Fritzbox`.

## Prometheus

The configuration for prometheus is located in `prometheus/prometheus.yml`.

---

## Contribute
Pull Requests are gladly welcome! 
Nevertheless, please don't forget to add an issue and connect it to your pull requests. This is very helpful to understand what kind of issue the PR is going to solve.

## Credits

Thanks to all contributors and everybody provided feedback.

- FRITZ!Box Smarthome exporter for prometheus: https://github.com/jayme-github/fritzbox_smarthome_exporter
- Prometheus: https://hub.docker.com/r/prom/prometheus
- Grafana: https://hub.docker.com/r/grafana/grafana

## Disclaimer

This project has no connection with the manufacturer AVM and was not sponsored. This is a private project that arose out of motivation.

If this project violates the terms of use or use cases, please contact me directly to find a solution.
