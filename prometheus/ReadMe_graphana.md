# Integrate and Visualize Prometheus Metrics on Grafana

## Install and Configure Grafana

sudo apt-get install -y apt-transport-https software-properties-common
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update -y
sudo apt-get install grafana -y
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server.service

**To remove**
sudo apt-get remove grafana
sudo rm -i /etc/apt/sources.list.d/grafana.list

Access Grafana Dashboard on http://<ip>:3000  admin/admin

Linux Hosts Metrics | Base
https://grafana.com/grafana/dashboards/10180
UID :  ov0oEgdik

ids::
10180
7353
3662



https://prometheus.io/docs/guides/basic-auth   =>> SECURING PROMETHEUS API AND UI ENDPOINTS USING BASIC AUTH

Create the data source-from where metrics need to be drived...
Select data source as prometheus..
Create the dashboards..
install the stress and apply the load and check the cpu usage
stress --cpu 8 --io 4 --vm 2 --vm-bytes 128M --timout 5m

Note:: the query runs from the graphana dashboard will run that query on the prometheus server
queries
************
100 - (avg by (instance) (irate(node_cpu_seconds_total{job="prometheus_webservers",mode='idle'}[5m])) * 100)

