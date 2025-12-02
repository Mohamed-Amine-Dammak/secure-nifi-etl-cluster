# Secure Apache NiFi Multi-Node Cluster & ETL Pipeline (MySQL → HDFS)

**Internship Project – Data Engineering & Automation**

This project deploys a secure multi-node Apache NiFi 1.26.0 cluster using Ansible.  
It includes a complete ETL pipeline that extracts data from MySQL and loads it into HDFS.

### Key Features
- Automated deployment with Ansible
- Secure communication (TLS/HTTPS)
- ZooKeeper for cluster coordination
- Distributed ETL pipeline (MySQL → NiFi → HDFS)
- Prevents filename conflicts in HDFS

### Technologies
Apache NiFi 1.26.0, ZooKeeper, Ansible, MySQL, Hadoop HDFS, Java 11+


## Prerequisites
- Ansible on control node (SSH access as user master1)
- ZooKeeper running on master1, master2, master3 (port 2181)
- MySQL database with table `nifidemo.personnes`
- HDFS directory `/user/nifi/mysql-json` created
- Java 11+ on all nodes

## Quick Start

### 1. Deploy the NiFi Cluster
```bash
ansible-playbook -i inventory.ini playbook.yml

2. Access NiFi UI
https://slave1:8443/nifi
https://slave2:8443/nifi

Get username/password:

Bash
grep "Generated" /opt/nifi-1.26.0/logs/nifi-app.log
3. ETL Pipeline (MySQL → HDFS)
Import the pipeline from the template in the docs folder
(or follow docs/Cookbook-Ansible-et-du-scenario-dingestion-NiFi-multi-noeuds-1.docx).

Processors:
QueryDatabaseTable → ConvertAvroToJSON → PutHDFS

Recommended filename in PutHDFS:
${hostname}-${now():format('yyyyMMddHHmmssSSS')}-${UUID()}.json

Troubleshooting
Check status:

Bash
/opt/nifi-1.26.0/bin/nifi.sh status
View logs:

Bash
tail -f /opt/nifi-1.26.0/logs/nifi-app.log
Check HDFS files:

Bash
hdfs dfs -ls /user/nifi/mysql-json
