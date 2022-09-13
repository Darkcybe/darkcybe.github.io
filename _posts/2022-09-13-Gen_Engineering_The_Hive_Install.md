---
title: TheHive4, Cortex, and MISP Server Installation
categories: [General IT and Security, Engineering]
tags: [the hive, cortex, MISP]
comments: true
---
"TheHive is a scalable 3-in-1 open source and free Security Incident Response Platform designed to make life easier for SOCs, CSIRTs, CERTs and any information security practitioner dealing with security incidents that need to be investigated and acted upon swiftly. It is the perfect companion to MISP. You can synchronize it with one or multiple MISP instances to start investigations out of MISP events. You can also export an investigation's results as a MISP event to help your peers detect and react to attacks you've dealt with. Additionally, when TheHive is used in conjunction with Cortex, security analysts and researchers can easily analyze tens if not hundred of observables." [The Hive](https://github.com/TheHive-Project/TheHive)

This post will run through the steps involved in the installation and configuration of The Hive 4.x and Cortex on a single server deployment. Additionally, MISP will also be installed on the same server.

# Hardware Dependencies

| Users |  CPU  |   RAM   | Storage |
|:-----:|:-----:|:-------:|:-------:|
|   <3  | 2vCPU |  4-8GB  |   50GB  |
|  <10  | 4vCPU |  8-16B  |  100GB  |
|  >10  | 8vCPU | 16-32GB |  200GB  |

# Installing TheHive4

1. TheHive requires Java OpenJDK version 8 or 11 (LTS) in order to load, although 8 is required to load the Cassandra nodes in the following database setup
2. Install the Apache Cassandra database, note that version 3.11.x is supported by TheHive with its repo added to the sources list. Cassandra is the backend database in which TheHive will write to.
   - Configure Cassandra by amending the cluster name and cassandra.yaml file. For this post, a single node is being created so the host IP address is the only parameter required in the IP options.
3. Install TheHive via APT package
   - Create a local file storage path for TheHive installation
4. Create an indexing directory for TheHive and change permissions.
   - > This may already be set during the installation process, however if not follow the steps to add the directories and change permissions. There should be 3 directories with all permissions set to `thehive:thehive` (databse, files, index). {: .prompt-info }
5. Configure `etc/thehive/application.conf`
   - Once complete, change ownership permissions for the `/opt/thp/thehive/files` directory
6. Start TheHive service
   - Once started, the hive can be access via the web-gui `http://YOUR_SERVER_ADDRESS:9000/`
   - The default admin user is `admin@thehive.local` with password `secret`. It is recommended to change the default password.

```bash
# Step 1
apt-get install -y openjdk-8-jre-headless
echo JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64" >> /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

# Step 2
curl -fsSL https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt update
sudo apt install cassandra

cqlsh localhost 9042
cqlsh> UPDATE system.local SET cluster_name = 'thp' where key='local';
nodetool flush

## /etc/cassandra/cassandra.yaml
cluster_name: 'thp'
listen_address: 'xx.xx.xx.xx' # address for nodes
rpc_address: 'xx.xx.xx.xx' # address for clients
seed_provider:
- class_name: org.apache.cassandra.locator.SimpleSeedProvider
    parameters:
    # Ex: "<ip1>,<ip2>,<ip3>"
    - seeds: 'xx.xx.xx.xx' # self for the first node
data_file_directories:
- '/var/lib/cassandra/data'
commitlog_directory: '/var/lib/cassandra/commitlog'
saved_caches_directory: '/var/lib/cassandra/saved_caches'
hints_directory:
- '/var/lib/cassandra/hints'

# Step 3
mkdir -p /opt/thp/thehive/files
curl https://raw.githubusercontent.com/TheHive-Project/TheHive/master/PGP-PUBLIC-KEY | sudo apt-key add -
echo 'deb https://deb.thehive-project.org release main' | sudo tee -a /etc/apt/sources.list.d/thehive-project.list
sudo apt-get update
sudo apt-get install thehive4

# Step 4
mkdir /opt/thp/thehive/index
chown -R thehive:thehive /opt/thp/thehive/index
chown -R thehive:thehive /opt/thp/thehive/files

# Step 5
db {
    provider: janusgraph
    janusgraph {
        storage {
            backend: cql
            hostname: ["127.0.0.1"] # seed node ip addresses
            cql {
                cluster-name: thp       # cluster name
                keyspace: thehive       # name of the keyspace
                }
            }
        }
    }
    ## Storage configuration
    storage {
        provider = localfs
        localfs.location = /opt/thp/thehive/files }

# Step 6
service thehive start
```
{: .nolineno }

# Installing Cortex
Cortex allows the automatic analysis of observables stored with a TheHive case. Examples are such things as IP reputation checks, VirusTotal checks, and intelligence scanning for IOCs. The developers behind TheHive created and maintain Cortex, making the linkage between the two seamless. Cortex works via API calls to various external sources. The following example outlines the steps to install and configure Cortex on the same server running TheHive.

1. Cortex requires Elasticsearch v7.x prior to installation, therefore the initial step is to add the Elasticsearch repo to the sources list and install the package.
   - Once the package has installed, configure the `elasticsearch.yml` file accordingly. Once edited, start the Elasticsearch service.
2. Install Cortex via APT package
   - TheHive repo should already be added to the APT sources list, however if not it can be added using the commands displayed.
   - If already added, simply run the cortex install command.
3. Configure Cortex
   - Create the Cortex secret key and apply it to the `/etc/cortex/application.conf` file.
   - Amend the Elasticsearch IP address
   - Start the Cortex service
     - Once started, the hive can be access via the web-gui `http://YOUR_SERVER_ADDRESS:9001/`
     - Set an admin username and password

```bash
# Step 1
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt install apt-transport-https
sudo apt update && sudo apt install elasticsearch

## Elasticsearch.yml
http.host: <IP ADDR>
cluster.name: hive
thread_pool.search.queue_size: 100000

## Service Restart
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service

# Step 2
curl https://raw.githubusercontent.com/TheHive-Project/TheHive/master/PGP-PUBLIC-KEY | sudo apt-key add -
echo 'deb https://deb.thehive-project.org release main' | sudo tee -a /etc/apt/sources.list.d/thehive-project.list
sudo apt-get update
sudo apt-get install cortex

# Step 3
## Application.conf
play.http.secret.key="$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 64 | head -n 1)"
    ## ElasticSearch
        search {
            # Name of the index
            index = cortex
            # ElasticSearch instance address.
            uri = "http://<IP ADDR>:9200"

## Service Restart
sudo systemctl enable cortex
sudo service cortex start
```

## Installing and Configuring Cotex Analyzers and Responders
1. Install pre-requisite python packages
2. Clone GibHub repo containing pre-build analyzers and responders
3. Install required packages for analyzers and responders
4. Add directory to `application.conf`

```bash
# Install Pre-requisuite packages
sudo apt-get install -y --no-install-recommends python2.7-dev python3-pip python3-dev ssdeep libfuzzy-dev libfuzzy2 libimage-exiftool-perl libmagic1 build-essential git libssl-dev
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python2 get-pip.py
sudo pip install -U pip setuptools && sudo pip3 install -U pip setuptools

# Clone Guthub Repo
cd /opt/cortex/
git clone https://github.com/TheHive-Project/Cortex-Analyzers

# Install pip packages
for I in $(find Cortex-Analyzers -name 'requirements.txt'); do sudo -H pip2 install -r $I; done && \
for I in $(find Cortex-Analyzers -name 'requirements.txt'); do sudo -H pip3 install -r $I || true; done

# Add analyzer and responder directories to /etc/cortex/application.conf
/path/to/directory/analyzer
/path/to/directory/responder
```
{: .nolineno }

# Installing MISP
"A threat intelligence platform for sharing, storing and correlating Indicators of Compromise of targeted attacks, threat intelligence, financial fraud information, vulnerability information or even counter-terrorism information. Discover how MISP is used today in multiple organisations. Not only to store, share, collaborate on cyber security indicators, malware analysis, but also to use the IoCs and information to detect and prevent attacks, frauds or threats against ICT infrastructures, organisations or people." [MISP](https://www.misp-project.org/)
1. As this instance of MISP is being installed on the same server hosting TheHive and Cortex, increase the memory assigned to the server by at least an additional 4GB of RAM and an adequate level of storage (100GB).
2. Download the install script provided by the team over at MISP and run the script to begin the install.
   - The install should largely be unattended, however there may be a popup indicating that a user account ‘misp’ needs to be created, enter ‘y’ to accept this.
3. Done!

```bash
wget -O /tmp/INSTALL.sh https://raw.githubusercontent.com/MISP/MISP/2.4/INSTALL/INSTALL.sh
bash /tmp/INSTALL.sh -A
```
{: .nolineno }

# Sources
- [TheHive Project Documentation](https://docs.thehive-project.org/thehive/)
- [TheHive, Cortex, and MISP](https://blog.thehive-project.org/2017/06/19/thehive-cortex-and-misp-how-they-all-fit-together/#:~:text=Cortex%20is%20our%20standalone%20analysis%20engine%20and%20a,case%20in%20TheHive%20%28or%20in%20an%20alternate%20SIRP%29.)
- [Cortex Guide](https://github.com/TheHive-Project/CortexDocs/blob/master/installation/install-guide.md)