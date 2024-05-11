# Grafana-Prometheous-Docker

### Docker installation
- sudo apt-get install docker.io  

### Prometheous installation
- wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz  
- tar xvfz prometheus-2.52.0.linux-amd64.tar.gz  
- cd prometheus-2.52.0.linux-amd64  
- vi prometheus.yml  
-  ./prometheus --config.file=./prometheus.yml = Start the prometheus  

### Node-exporter installtion
- wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz  
- tar xvfz node_exporter-1.8.0.linux-amd64.tar.gz  
- cd node_exporter-1.8.0.linux-amd64  
- ./node_exporter = Start the node_exporter  

### Create Service of node_exporter and prometheus
- Create 2 files
1. prometheus.service file
   - path : cd /etc/systemd/system/  
   - code :  
```
    [Unit]
    Description=Prometheus Server
    Documentation=https://prometheus.io/docs/introduction/overview/
    After=network-online.target
    
    [Service]
    User=root
    Restart=on-failure
    ExecStart=/home/ubuntu/prometheus-2.52.0.linux-amd64/prometheus \
      --config.file=/home/ubuntu/prometheus-2.52.0.linux-amd64/prometheus.yml \
    
    [Install]
    WantedBy=multi-user.target
```
3. node_exporter.service file
   - path : cd /etc/systemd/system/  
   - code
```
      [Unit]
      Description=Node Exporter
      
      [Service]
      User=root
      Type=simple
      ExecStart=/home/ubuntu/node_exporter-1.8.0.linux-amd64/node_exporter
      
      [Install]
      WantedBy=multi-user.target
```

### Check configuration
- To check all configyration in promethues is right or not
   - command : promtool check config /etc/prometheus/prometheus.yml
- To check rules
   - command : sudo promtool check rules /etc/prometheus/prometheus_rules.yml   

- Need to add file in prometheus.yml file 
```
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"
    - "prometheus_rules.yml"

```