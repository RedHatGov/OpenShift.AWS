# OpenShift.AWS

`OpenShift.AWS` is a ansible playbook to install a OpenShift Enterprise 3.2 in AWS. 
Default number of nodes is `1` master and `2` slave nodes. All variables are stored in the `group_vars/all` file. This includes secrets like AWS API keys and Red Hat subscription info. This playbook also assumes that you will be using your own domain name and have it registered already in AWS. This allows the playbook to use subdomains for all OSE nodes, `master.ose.<yourdomain>.io`. 

# Dependencies

## Terraform
Terraform needs to be installed locally

OSX

```
brew install terraform
```

## AWS
This playbook creates a ssh key from AWS and uses it to ssh into the nodes and configure them. This key is placed in `/tmp/terraform/<key_name>`. 

## Setup Var Files

OpenShift & AWS settings

`group_vars/all`

```
# Domain Name you own
domain_name: ""
zone_id: ""
ssh_keyname: "openshift"
region: "us-east-1"

# OpenShift
ose_nodes: "2"
master_ami_type: "c3.large"
node_ami_type: "c3.large"

# AWS API Keys for terraform.tfvars file
aws_access_key: ""
aws_secret_key: ""

# Subscribe your nodes to Red Hat
username: ""
password: ""
pool_id: ""
```


# Usage


## Provision in AWS 
`ansible-playbook -i inventory site.yml`

## Destroy the Cluster & AWS resources

```
cd /tmp/terraform
terraform destroy
```

# Demo

Scale up pods in console

```
oc login
oc get pods
oc delete pod nodejs-example-1-02re8
```

View status and configuration

```
oc config view
oc status