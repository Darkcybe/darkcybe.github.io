---
title: Installing and Configuring Graylog
categories: [General IT and Security,Engineering]
tag: [graylog]
comments: true
---

## Overview

Graylog provides answers to your team’s security, application, and IT infrastructure questions by enabling you to combine, enrich, correlate, query, and visualize all your log data in one place.

## Installing Graylog 5.0 on Ubuntu 20.04

### **Prerequisites**

Graylog 5.0 requires the following to maintain compatibility with its software dependencies:

- OpenJDK 17 (embedded in the 5.0 installation file)
- Elasticsearch 7.10.2 ***OR*** OpenSearch 2.x
- MongoDB (5.x or 6.x)

### Installing MongoDB 6.1

1. Import the public key used by APT and add the repository to the APT sources file. Once completed, MongoDB can be installed after updating the package database.

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

2. Enable the MongoDB service to start at boot

```bash
sudo systemctl daemon-reload
sudo systemctl enable mongod.service
sudo systemctl restart mongod.service
sudo systemctl status mongod.service # Check that the service is active
```

### Installing OpenSearch 2.4.1

Graylog supports running on either Elasticsearch or OpenSearch. The preference is entirely up to you, however I choose to run OpenSearch as it is opensource and includes a few enhanced features over ElasticSearch 7.10.2. For a more detailed reference on the difference between the two, see the referenced blog post.

1. Check the `vm.max_map_count` setting to ensure that it is set to at least 262144 and disable memory paging and swapping to improve performance.

```bash
cat /proc/sys/vm/max_map_count # Checks the current max_map_count
sudo nano /etc/sysctl.conf
	vm.max_map_count=262144 # Enter this line to sysctl.conf
sudo sysctl -p # Reload the new setting
sudo swapoff -a
```

2. Create a User and Group for the OpenSearch app and create directories to store the application data and log files.

```bash
sudo adduser --system --disabled-password --disabled-login --home /var/empty --no-create-home --quiet --force-badname --group opensearch
sudo mkdir -p /graylog/opensearch/data
sudo mkdir /var/log/opensearch
```

3. Download and extract the appropriate tar.gz archive, in this case it is the x64 version. Move the extracted data into the previously created directory.

```bash
wget https://artifacts.opensearch.org/releases/bundle/opensearch/2.4.1/opensearch-2.4.1-linux-x64.tar.gz
tar -xvf opensearch-2.4.1-linux-x64.tar.gz
sudo mv opensearch-2.4.1/* /graylog/opensearch/
sudo rm -rf opensearch* # Clean up the downloaded tar archive
```

4. Set appropriate permissions for the OpenSearch app for the previously created opensearch user and group. Create an empty log file under the same user account.

```bash
sudo chown -R opensearch:opensearch /graylog/opensearch/
sudo chown -R opensearch:opensearch /var/log/opensearch
sudo chmod -R 2750 /graylog/opensearch/
sudo chmod -R 2750 /var/log/opensearch
sudo -u opensearch touch /var/log/opensearch/graylog.log
```

5. Create a system service

```bash
sudo su
cat > /etc/systemd/system/opensearch.service <<EOF
[Unit]
Description=Opensearch
Documentation=https://opensearch.org/docs/latest
Requires=network.target remote-fs.target
After=network.target remote-fs.target
ConditionPathExists=/graylog/opensearch
ConditionPathExists=/graylog/opensearch/data
[Service]
Environment=OPENSEARCH_HOME=/graylog/opensearch
Environment=OPENSEARCH_PATH_CONF=/graylog/opensearch/config
ReadWritePaths=/var/log/opensearch
User=opensearch
Group=opensearch
WorkingDirectory=/graylog/opensearch
ExecStart=/graylog/opensearch/bin/opensearch
# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65535
# Specifies the maximum number of processes
LimitNPROC=4096
# Specifies the maximum size of virtual memory
LimitAS=infinity
# Specifies the maximum file size
LimitFSIZE=infinity
# Disable timeout logic and wait until process is stopped
TimeoutStopSec=0
# SIGTERM signal is used to stop the Java process
KillSignal=SIGTERM
# Send the signal only to the JVM rather than its control group
KillMode=process
# Java process is never killed
SendSIGKILL=no
# When a JVM receives a SIGTERM signal it exits with code 143
SuccessExitStatus=143
# Allow a slow startup before the systemd notifier module kicks in to extend the timeout
TimeoutStartSec=180
[Install]
WantedBy=multi-user.target
EOF
```

### Configuring OpenSearch for Graylog

1. Open the pre-completed YAML file and update the following fields, remove pound characters as required. This configuration is modified from the recommendations set by Graylog as its a single-node deployment.

```bash
sudo nano /graylog/opensearch/config/opensearch.yml

# Insert the below configuration
cluster.name: graylog
node.name: ${HOSTNAME}
path.data: /graylog/opensearch/data
path.logs: /var/log/opensearch
network.host: <IP>
http.port: 9200

# discovery.seed_hosts: 
# cluster.initial_master_nodes:
discovery.type: single-node 
action.auto_create_index: false
plugins.security.disabled: true
```

2. Specify initial and maximum JVM heap sizes to half of the host systems memory allocation. This can be increased post configuration of Graylog should you require more memory assigned. After setting the JVM heap sizes, specify the environment path for the setting.

```bash
sudo nano /graylog/opensearch/config/jvm.options

# These settings represent 8GB allocations
-Xms8g
-Xmx8g

export OPENSEARCH_JAVA_HOME=/graylog/opensearch/jdk
```

3. Enable the system service to run upon system boot

```bash
sudo systemctl daemon-reload
sudo systemctl enable opensearch.service
sudo systemctl start opensearch.service
sudo systemctl status opensearch.service # Check that the service is active
```

### Installing Graylog

1. Download and install the Graylog package from the legitimate repository. Once completed, the server components can be installed via the APT package.

```bash
wget https://packages.graylog2.org/repo/packages/graylog-5.0-repository_latest.deb
sudo dpkg -i graylog-5.0-repository_latest.deb
sudo apt-get update && sudo apt-get install graylog-server
rm graylog* # clean up download
```

### Configuring Graylog

1. Graylog is configured using a YAML file within the `/etc/graylog/server/` directory and requires configuration in order to start. Initially, a root password hash using SHA2 is required. Make note of the password and hash as both are required to be entered into the configuration file. Note that the password must be a minimum of 16 characters.

```bash
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1

sudo nano /etc/graylog/server/server.conf

# Configure the details as required
password_secret = <PASSWORD> # Min 16 Characters (at least 64 Recommended)
root_username = <USERNAME>
root_password_sha2 = <ROOT PASSWORD HASH> # Different to password_secret - memorize plain-text equivilent)
http_bind_address = <IP>:9000 # Set to host IP
```

2. Enable the system service to run upon system boot

```bash
sudo systemctl daemon-reload
sudo systemctl enable graylog-server.service
sudo systemctl start graylog-server.service
sudo systemctl status graylog-server.service # Check that the service is active
```

3. Graylog can now be access via web browser by navigating to `http://<IP>:9000`

## Resources
- [Ubuntu installation (graylog.org)](https://go2docs.graylog.org/5-0/downloading_and_installing_graylog/ubuntu_installation.html)
- [Install MongoDB Community Edition on Ubuntu — MongoDB Manual](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#:~:text=Install%20MongoDB%20Community%20Edition%201%20Import%20the%20public,database.%20...%204%20Install%20the%20MongoDB%20packages.%20)
- [Elasticsearch vs OpenSearch: How They Differ? - Webuters](https://www.webuters.com/elasticsearch-vs-opensearch-how-they-differ)
- [Install OpenSearch - OpenSearch documentation](https://opensearch.org/docs/2.4/install-and-configure/install-opensearch/index/)
- [Sending in log data (graylog.org)](https://go2docs.graylog.org/5-0/getting_in_log_data/getting_in_log_data.html)