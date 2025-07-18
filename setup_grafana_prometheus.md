# Grafana + Prometheus + Node Exporter Setup on AWS

## Overview

This guide documents how to set up monitoring for AWS EC2 instances using:

- **Grafana**: Visualization dashboard
- **Prometheus**: Metrics collection and storage
- **Node Exporter**: Export system metrics from EC2 instances

---

## Prerequisites

- AWS EC2 Ubuntu instances (one for Grafana/Prometheus server, others to monitor)
- SSH access configured
- Security groups allowing required ports:
  - Grafana: 3000
  - Prometheus: 9090
  - Node Exporter: 9100

---

## Step 1: Install Node Exporter on EC2 to Monitor

```bash
# Download Node Exporter
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-1.9.1.linux-amd64.tar.gz

# Extract and move binary
tar -xvzf node_exporter-1.9.1.linux-amd64.tar.gz
sudo mv node_exporter-1.9.1.linux-amd64/node_exporter /usr/local/bin/

# Create systemd service file
sudo nano /etc/systemd/system/node_exporter.service
# Paste the following content:
# [Unit]
# Description=Prometheus Node Exporter
# After=network.target
#
# [Service]
# User=ubuntu
# ExecStart=/usr/local/bin/node_exporter
#
# [Install]
# WantedBy=default.target

# Enable and start the service
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
