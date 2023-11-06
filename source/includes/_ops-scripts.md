# ops-scripts

## Create Cluster - AWS

```shell
$ mkdir $DEVOPS_HOME/cluster/zc2-dtran-qa.us-west-1.ec2.qa-cloud.redislabs.com/
$ cp $DEVOPS_HOME/bin/sample_config.sh $DEVOPS_HOME/cluster/zc2-dtran-qa.us-west-1.ec2.qa-cloud.redislabs.com/config.sh
```
> sample_config.sh

```shell
# Frontend DNS group name which cluster will get registered under
DNS_FRONTEND_GROUP=qa
UBUNTU_RELEASE=bionic-18.04
# Instance type to be use
FLAVOR=m5.large
# Available local drives to be use. In case of more than one drive, the ephemeral drive will get build as RAID 0 from all local drives
EPHEMERAL_DRIVES=/dev/sdb,/dev/sdc
# Availability zone for next new instance while using the add_replace_node.sh (note : not for create_all_nodes.sh)
ADD_REPLACE_NODE_AZ=us-east-1a
# Cluster region
REGION=us-east-1
# Number of instances to launh while using create_all_nodes.sh (note : all instances will be the same type)
INITIAL_CLUSTER_SIZE=1
# /ebs volume size. If remarked, /ebs size will be X5 of RAM size.
#EBS_SIZE=50
# Create cluster as ROF
USE_ROF=no
# Create cluster with MultiProxy on
USE_MP=yes
# Create cluster which is part of CRDT
CRDT=no
# Multi zone cluster enforcement
RACK_AWARE=no
# RLEC version to be use
RLEC_VERSION="6.2.xx-xx"
# Bootstrap cluster after create_all_nodes.sh. Should be "no" in case of recovery.
RLEC_BOOTSTRAP=yes

# If yes, maximum statistics history will be "Hour", if no, maximum statistics history will be "Year"
SERVICE=yes

# VPC based configuration
USE_VPC=yes
VPC_CIDR="10.0.0.0/16"

# If full, uses those existing ebs volumes for each specified node here
NODE_1_VOLUME_ID=

# This will replace existing AVAILABILITY_ZONE. For each AZ a CIDR can be supplied for VPC by using AZ_CIDR_<az name without seperators>. Not defining AZ_CIDR a az in VPC will fail the init
AVAILABILITY_ZONE_LIST=( us-east-1a )
AZ_CIDR_useast1a="10.0.0.0/24"

# A list of variables setting nodes availability zones. Do note that if not mentioned, the create_all_nodes will iterate over the zones list. This will not affect add replace node az
NODE1_AZ=us-east-1a

# NETWORK ADDRESSES PARAMETERS
# IP's/Subnet to be allow for SSH access
PUBLIC_NETWORK_ADDRESS_1="0.0.0.0/0"
# Internal cluster subnet (to be open in iptables)
INTERNAL_NETWORK_ADDRESS_1="10.0.0.0/0"
# IP's/Subnet to be allow for Cluster Managment (port 8443)
CM_FRONTEND_NETWORK_ADDRESS_1="0.0.0.0/0"
# IP's/Subnet to be allow for REST API (ports 8080 & 9443)
SM_NETWORK_ADDRESS_1="0.0.0.0/0"
# IP's/Subnet to be allow for zabbix agent (port 10050)
ZABBIX_NETWORK_ADDRESS_1="13.59.16.120/32"
# IP's/Subnet to be allow for prometheus agent (port 8070) - the new prometheus GKE NAT IP.
PROMETHEUS_NETWORK_ADDRESS_1="0.0.0.0/0"
# IP's/Subnet to be allow for endpoint access (to be define in cluster security group)
ENDPOINT_CLIENTS_NETWORK_ADDRESS_SG_1="0.0.0.0/0"
# IP's/Subnet to be allow for endpoint access (to be define in nodes iptables rules, note : should be empty on cluster that is managed by SM)
ENDPOINT_CLIENTS_NETWORK_ADDRESS_RULE_1=""

# ZABBIX RELATED PARAMETERS
ZABBIX_SERVER="zabbix6-qa.redislabs.com"
```
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/471793685/Create+Cluster+-+AWS)

**Information Gathering**

* Onboarding Questionnaire
* Explicit customer confirmation of static IPs and CIDRs provided

**Configuration:**

* Populating the config.sh

**Provision:**

* Creating the instances and bootstrap
* Adding SSH sessions to SecureCRT to access cluster
* Adding host objects to Zabbix for monitoring
* Adding to SM/BackOffice to allow access to Cloud service

**Document**

* Cluster notes - contains details and configuration specific to the cluster
* CMDB - tracks all infrastructure changes and service level configurations

[Recording](https://drive.google.com/file/d/1Y5iYr72w1P3NrZAHh7Hpa316-mcaEtot/view?usp=drive_link) 

## Add New Node - AWS
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438305219/Add+a+New+Node+-+AWS)

## Replace Faulty Node - AWS
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438600189/Replace+Faulty+Node+-+AWS)

## Replace Healthy Node - AWS
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/470089741/Replace+Healthy+Node+-+AWS)

## Remove Node - AWS
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438338070/Remove+a+Node+-+AWS)

## Remove Cluster - AWS
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
