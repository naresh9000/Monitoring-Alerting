prometheus Installation & configuration
***************************************

Reference:: https://devopscube.com/install-configure-prometheus-linux/

            https://prometheus.io/download/

**------------------------------------------------------------------------------------------**
For running locally
docker run --name prometheus -d -p 127.0.0.1:9090:9090 prom/prometheus
Reachable at http://localhost:9090/.

**-------------------------------------------------------------------------------------------**
Deploy t2.medium ubuntu 22.04 server.

**Perform following steps to copy binaries and permissions.**

sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz -P /tmp
cd /tmp
tar xvf /tmp/prometheus-2.47.1.linux-amd64.tar.gz

**The provided commands are used to copy the Prometheus binary and related files to appropriate locations and set the ownership to the prometheus user and group:**

sudo cp /tmp/prometheus-2.47.1.linux-amd64/prometheus /usr/local/bin/
sudo cp /tmp/prometheus-2.47.1.linux-amd64/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo cp -r /tmp/prometheus-2.47.1.linux-amd64/consoles /etc/prometheus
sudo cp -r /tmp/prometheus-2.47.1.linux-amd64/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus  /etc/prometheus/consoles
sudo chown -R prometheus:prometheus  /etc/prometheus/console_libraries

ls -ltr /usr/local/bin/ | grep prom

**optional**
setting the ownership for directories..and for data storage..
sudo mkdir -p /data
sudo chown -R prometheus:prometheus /data/


3.sudo nano /etc/prometheus/prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporter'
        scrape_interval: 5s
        static_configs:
        - targets: ['localhost:9100']
  - job_name: 'prometheus_public_self_system_metrics'
        scrape_interval: 5s
        static_configs:
        - targets: ['ec2-18-60-216-32.ap-south-2.compute.amazonaws.com:9090']
  - job_name: 'jenkins'
    metrics_path: '/prometheus'
    static_configs:
      - targets: ['<your-jenkins-ip>:<your-jenkins-port>']

4.	sudo nano /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Restart=on-failure
RestartSec=5s
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file=/etc/prometheus/prometheus.yml \
--storage.tsdb.path=/data \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries \
--web.listen-address=0.0.0.0:9090 \
--web.enable-lifecycle \
--web.enable-admin-api \
--log.level=info

[Install]
WantedBy=multi-user.target


5.	Start & Check Prometheus Status
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus


**Runs on 9090 port**

If you want to restart prometheus service
sudo systemctl daemon-reload
sudo systemctl restart prometheus


Reload Prometheus Config Without Restarting Prometheus:
curl -s -XPOST localhost:9090/-/reload

**journalctl -u prometheus**: View the logs related to the Prometheus service
netstat -tulnp |grep -w ':9090'
(TCP, UDP, Listening, Numeric, and Process information.)
 ** :::9090 ** means that the socket is listening for incoming connections on port 9090 for all available IPv6 addresses on the system.

**********************************************************************************************************
**Installing & Configuring Node Exporter:**

sudo useradd --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz -P /tmp
cd /tmp
tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo cp /tmp/node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

**-------------------------------------------------------------------------**
nano /lib/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter --collector.logind

[Install]
WantedBy=multi-user.target

**------------------------------------------------------**
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter

**Runs on 9100 port**
**--------------------------------------------------------**

Reload Prometheus Config Without Restarting Prometheus:
way to apply changes to the configuration files dynamically without downtime of the entire service.
useful in automated deployment scenarios or when making changes to Prometheus configurations remotely

curl -s -XPOST localhost:9090/-/reload
or
sudo systemctl restart prometheus   => it reloads the entire servcie having down time for metric collection

**-------------------------------------------------------------------------------------------------------------**
Launch two t2.micro machines install the node_exporter.

Add the below lines in promethesu.yaml file in prometheus server.

```
sudo nano /etc/prometheus/prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus_private'
        scrape_interval: 5s
        static_configs:
        - targets: ['localhost:9090']
  - job_name: 'prometheus_public'
        scrape_interval: 5s
        static_configs:
        - targets: ['ec2-18-60-216-32.ap-south-2.compute.amazonaws.com:9090']
  - job_name: 'prometheus_node_exporter'
        scrape_interval: 5s
        static_configs:
        - targets: ['ec2-18-60-216-32.ap-south-2.compute.amazonaws.com:9100']
  - job_name: 'prometheus_webservers'
        scrape_interval: 5s
        static_configs:
        - targets: ['<machine-1-ip>:9100','<machine-2-ip>:9100']

```
curl -s -XPOST localhost:9090/-/reload
