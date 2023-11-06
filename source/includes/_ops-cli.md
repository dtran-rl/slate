# ops-cli

## Create Cluster - GCP

> gcp_defaults.json

```shell
{
  "provider.region": "us-central1",
  "provider.creds_file": "/etc/ops-cli/creds/redislabs-service-gpc-creds-file.json",
  "provider.project": "",
  "version": "",
  "node_default.availability_zone": "us-central1-a",
  "node_default.machine_type": "n1-standard-1",
  "node_default.ephemeral_storage": true,
  "node_default.platform": "xenial",
  "node_default.ephemeral_size": 500,
  "node_default.use_nvme": false,
  "node_default.persist_disk_size": 0,
  "additional_dns_names": [],
  "dns_group": "qa",
  "network_addresses.public": [
    "0.0.0.0/0"
  ],
  "network_addresses.internal": [
    "172.16.100.0/24"
  ],
  "network_addresses.access_cm": [
    "0.0.0.0/0"
  ],
  "network_addresses.sm_servers": [
    "0.0.0.0/0"
  ],
  "network_addresses.clients_network": [
    "0.0.0.0/0"
  ],
  "network_addresses.clients_internal": [],
  "network_addresses.zabbix_servers": [
    "0.0.0.0/0"
  ],
  "network_addresses.prometheus_servers": [
    "0.0.0.0/0"
  ],
  "bootstrap": true,
  "logs_on_root": false,
  "service": true,
  "features.redis_on_flash": true,
  "features.multiproxy": true,
  "features.rack_aware": false,
  "zabbix_server": "zabbix6-qa.redislabs.com",
  "cloud_cert_service": true
}
```
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/481689674/Create+Cluster+-+GCP)

**Information Gathering**

* Onboarding Questionnaire
* Explicit customer confirmation of static IPs and CIDRs provided

**Configuration:**

* Populating the gcp_defaults.json

**Provision:**

* Creating the instances and bootstrap
* Adding SSH sessions to SecureCRT to access cluster
* Adding host objects to Zabbix for monitoring
* Adding to SM/BackOffice to allow access to Cloud service

**Document**

* Cluster notes - contains details and configuration specific to the cluster
* CMDB - tracks all infrastructure changes and service level configurations

[Recording](https://drive.google.com/file/d/1Y5iYr72w1P3NrZAHh7Hpa316-mcaEtot/view?usp=drive_link) 

## Add New Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438305219/Add+a+New+Node+-+AWS)

## Replace Faulty Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438600189/Replace+Faulty+Node+-+AWS)

## Replace Healthy Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/470089741/Replace+Healthy+Node+-+AWS)

## Remove Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438338070/Remove+a+Node+-+AWS)

## Remove Cluster
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/471793668/Remove+Cluster+-+AWS)

## Exercise #1

1. Create Single AZ GCP Cluster - us-west1
    * Cluster version - 6.0.12-58
    * OS version - bionic
2. Add to BO CGs + Create 2+ DBs +R (if region is available in SM, otherwise create DBs in CM)
3. Replace 1 node with shards as Healthy
4. Replace 1 node with shards as Faulty
5. Perform cluster upgrade to Latest RCE version
6. Remove Cluster + Cleanup
 
## Exercise #2 

1. Create MZ GCP Cluster - us-west1
2. Add 2 data nodes (maintain balanced AZs)
3. Add to BO CGs + Create DBs (Use CM if needed)
4. Replace Healthy + Faulty
5. Perform cluster upgrade to Latest RCE version
6. Remove Cluster + Cleanup
7. Configure VPC peering with each other (Before or After upgrade is fine)
    * Once with gcloud
    * Once with GCP Console

[Recording](https://drive.google.com/drive/folders/1XMOmncjGUZq6c_Y9Gp3gRis-OESm8s7r)
