# ops-scripts

## Create Cluster

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
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/471793685/Create+Cluster+-+AWS).

Information Gathering
- Onboarding Questionnaire
- Explicit customer confirmation of static IPs and CIDRs provided

Configuration:
- Populating the config.sh


## Add New Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438305219/Add+a+New+Node+-+AWS).

## Replace Faulty Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438600189/Replace+Faulty+Node+-+AWS).

## Replace Healthy Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/470089741/Replace+Healthy+Node+-+AWS).

## Remove Node
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/438338070/Remove+a+Node+-+AWS)

## Remove Cluster
[Procedure](https://redislabs.atlassian.net/wiki/spaces/DevOps/pages/471793668/Remove+Cluster+-+AWS).

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: meowmeowmeow"
```