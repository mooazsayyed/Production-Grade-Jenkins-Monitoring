# Production Grade Jenkins Monitoring with Prometheus, Grafana, InfluxDB

This guide walks you through setting up a Jenkins monitoring stack using Docker only, with data visualization in Grafana, data collection in InfluxDB, and Jenkins pipelines sending metrics to Influx.

## 📦 Stack Components

_Jenkins: CI/CD pipelines<br>
InfluxDB: Time-series database for storing Jenkins metrics<br>
Prometheus: (Optional in this stack)<br>
Grafana: Dashboards for Jenkins metrics visualization_<br>

## ⚙️ Setup Using Docker

### 1. Prometheus 
```
docker run -d --name prometheus-container \
  -v /home/ubuntu/prometheus.yml:/etc/prometheus/prometheus.yml \
  -e TZ=UTC \
  -p 9090:9090 \
  ubuntu/prometheus:2.33-22.04_beta
```
_ℹ️Adjust prometheus.yml as per your job scraping requirements._

### 2. Grafana
``docker run -d -p 8086:8086 --name influxdb2 influxdb:1.8.6-alpine``

### 3. InfluxDB (v1.8)
``docker run -d -p 8086:8086 --name influxdb2 influxdb:1.8.6-alpine``

### 4. Jenkins
``docker run -d -p 8080:8080 --name jenkins jenkins/jenkins:lts-jdk11``

## 🔗 InfluxDB Setup for Jenkins Metrics

### Access InfluxDB and Create Database
``docker exec -it influxdb2 bash
influx
``
<br>

__Run the following commands inside the Influx shell:__

```
CREATE DATABASE "jenkins" WITH DURATION 1825d REPLICATION 1 NAME "jenkins-retention";
SHOW DATABASES;
USE jenkins;
```
## 🔌 *Jenkins Configuration*

✅ Required Plugins
Install these plugins from Jenkins Plugin Manager:

_Prometheus Plugin<br>
InfluxDB Plugin<br>
Job and Stage Monitoring Plugin_

*SETUP*

<img width="1731" height="857" alt="image" src="https://github.com/user-attachments/assets/30528c0b-fe41-49da-8748-555609399713" />


## 🔧 *Global Configuration*
Go to Manage Jenkins → Configure System

Scroll to InfluxDB Target

Add:<br>

URL: http://<influx_host>:8086
Database: jenkins
Custom Prefix: jenkins_data (if needed)
Retention Policy: jenkins-retention

# 📊 Jenkins + Grafana + InfluxDB Dashboard

This project helps you visualize Jenkins pipeline data using Grafana dashboards powered by InfluxDB.

---

## 🧪 Generate Sample Data

Create folders and multiple pipelines.

Run jobs with various outcomes:

- ✅ **Success**
- ❌ **Failure**
- ⚠️ **Unstable**
- 🛑 **Aborted**

---

## 📊 Grafana Dashboard Design

### 🔍 Filtering & Visualization

Use templated variables for filtering:

- `job`, `folder`, `status`

Create dropdowns using Influx queries like:

```sql
SHOW TAG VALUES FROM job WITH KEY = "owner";
SHOW TAG VALUES FROM job WITH KEY = repo WHERE "owner" =~ /^($folder)$/;
```
# COMPLETE DASHBOARD 

![Jenkins-Monitoring](https://github.com/user-attachments/assets/4deb1963-c092-41b0-a448-b288bbbd29e7)


#### _*CHECK THE ./Influxdb-queries file for influx quereies*_

📬 _Feel free to contribute or suggest improvements!_
