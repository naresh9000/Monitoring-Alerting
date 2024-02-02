
Reference::
https://linuxhint.com/install-configure-prometheus-alert-manager-ubuntu/
https://samber.github.io/awesome-prometheus-alerts/   for all rules for devops --have many options


# installation and configuration

wget https://github.com/prometheus/alertmanager/releases/download/v0.26.0/alertmanager-0.26.0.linux-amd64.tar.gz /tmp
cd /tmp
tar -xvzf alertmanager-0.26.0.linux-amd64.tar.gz
mv alertmanager-0.26.0.linux-amd64/alertmanager /usr/local/bin/
mkdir /etc/alertmanager/
sudo chown prometheus:prometheus /etc/alertmanager/


Create the 2 step verification in gmail..got to security and generate the **auth password**

Create the alertmanager.yml file
 sudo nano /etc/alertmanager/alertmanager.yml

 
UPdate prometheus.yml file with the alertmanager configuration as below.
```
  alertmanagers:
  - static_configs:
    - targets: ['<alertmanager-public-ip>:9093']

rule_files: alerting:

- alert_rules.yml

<insert prometheus.yml config file data >

```

Create alertmanager service:
 sudo nano /etc/systemd/system/alertmanager.service

```
[Unit]
Description=Alertmanager for prometheus

[Service]
Restart=always
User=prometheus
ExecStart=/usr/local/bin/alertmanager --config.file=/etc/alertmanager/alertmanager.yml --storage.path=/etc/alertmanager/       	 
ExecReload=/bin/kill -HUP $MAINPID
TimeoutStopSec=20s
SendSIGKILL=no

[Install]
WantedBy=multi-user.target
```

sudo systemctl daemon-reload
sudo systemctl start alertmanager.service
sudo systemctl enable alertmanager.service
sudo systemctl status alertmanager.service

Apply the stress on servers...check the gmail
stress  --vm 2 --vm-bytes 256M --timeout 10m 
