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

1. Create single AZ AWS Cluster in us-west
    * Cluster version - `6.0.12-58`
    * OS Version - `Ubuntu 18`
2. Add to Zabbix
3. Add data nodes of different instance type
4. Add to BackOffice + dedicate to your account
5. Create a DB + Replication
6. Upgrade from `6.0.12-58` to `6.2.18-49`
7. Replace node:2 as faulty
    * Observe the cnm_exec.log, cluster_wd.log, node_wd.log
8. Remove cluster + cleanup

## Exercise #2

1. Create MZ AWS Cluster in us-east-1
2. Configure as Off-SM Cluster
    * Can skip mail server step
3. Add to Zabbix
4. Create a DB with 4+ master shards using Database Clustering in CM
5. Scale up a data node using Replace as Healthy
6. Upgrade from `6.0.12-58` to `6.2.18-49`
7. Remove cluster + cleanup

**Notes:**

* When re-using the cluster UID/FQDN, some clean up is required
    * Removing the data folder (.initialized file), $DEVOPS_HOME/cluster/YOUR_CLUSTER_FQDN/data
    * Clearing VPC details in the config.sh
    * Chef environment cleanup, one-liner to get commands to run
    * ```for i in environment node "data bag" client ; do echo knife $i delete `knife $i list | grep CHANGE_TO_YOUR_FQDN` ; done```
    * Removing your cluster KeyPair from AWS viconsole
* When creating in us-west-1  observe the limitations of the region's instance types and AZ
    * May need to specify a different AZ (ex: us-west-1b)
    * When adding to BackOffice, remember to search for the correct Region
* When adding cluster to BackOffice Cluster Groups, your account ID may not populate in the autofill
    * Account's Clusters Page - add your account # as the Account's Default Zone and Account's Default Cluster
    * Account's Clusters Page - remove my account # from being dedicated to your cluster
* When removing your cluster from BackOffice CGs, you may see a message regarding the Cluster has BDBs
    * Verify the DB does not exist on your cluster or your SM UI (+subscription)
    * Safe to remove cluster from CGs
