# State

AWS offers many services to store state / data. Some are persistent, others are not.

## `state/client-sg` - Client Security Group

Some data stores are integrated into the VPC, others are only accessible via the AWS API. For VPC integration, you have to create a Client Security Group stack. The stack is used as a parent stack for ElastiCache, Elasticsearch, and RDS. To communicate with the data store from a EC2 instance, you have to attach the Client Security Group to the EC2 instance. The Security Group does not have any rules, but it marks traffic. The marked traffic is then allowed to enter the data store.

### Installation Guide
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)

## `state/dynamodb` - DynamoDB table

DynamoDB table with auto scaling for read and write capacity.

### Installation Guide
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=dynamodb-table&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/dynamodb.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `operations/alert.yaml` (recommended)

### Limitations
* No backup (see `operations/backup-dynamodb-native.yaml`)
* Encryption at rest with AWS managed CMK (customer managed is not supported)

## `state/elasticache-memcached` - ElastiCache memcached

Cluster of memcached nodes.

### Installation Guide
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. This templates depends on the `client-sg.yaml` template. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=memcached&param_ParentVPCStack=vpc-2azs&param_ParentClientStack=client-sg&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/elasticache-memcached.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)
* `state/client-sg.yaml` (**required**)
* `vpc/zone-*.yaml`
* `vpc/vpc-*-bastion.yaml`
* `operations/alert.yaml` (recommended)

### Limitations
* No backup
* No data replication (use as a in-memory cache only)
* No auto scaling

## `state/elasticsearch` - Elasticsearch

Cluster of Elasticsearch nodes.

### Installation Guide
1. Create [Service-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) for Elasticsearch: `aws --region us-east-1 iam create-service-linked-role --aws-service-name es.amazonaws.com`
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. This templates depends on the `client-sg.yaml` template. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=elasticsearch&param_ParentVPCStack=vpc-2azs&param_ParentClientStack=client-sg&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/elasticsearch.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)
* `state/client-sg.yaml` (**required**)
* `vpc/zone-*.yaml`
* `vpc/vpc-*-bastion.yaml`
* `operations/alert.yaml` (recommended)

### Limitations
* No auto scaling

## `state/rds-aurora` - RDS Aurora

Two node Aurora cluster for HA.

### Installation Guide
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. This templates depends on the `client-sg.yaml` template. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=rds-aurora&param_ParentVPCStack=vpc-2azs&param_ParentClientStack=client-sg&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/rds-aurora.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)
* `state/client-sg.yaml` (**required**)
* `vpc/zone-*.yaml`
* `vpc/vpc-*-bastion.yaml`
* `operations/alert.yaml` (recommended)

### Limitations
* No auto scaling

## `state/rds-mysql` - RDS MySQL

Multi-AZ MySQL for HA.

### Installation Guide
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. This templates depends on the `client-sg.yaml` template. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=rds-mysql&param_ParentVPCStack=vpc-2azs&param_ParentClientStack=client-sg&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/rds-mysql.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)
* `state/client-sg.yaml` (**required**)
* `vpc/zone-*.yaml`
* `vpc/vpc-*-bastion.yaml`
* `operations/alert.yaml` (recommended)

### Limitations
* No auto scaling

## `state/rds-postgres` - RDS Postgres

Multi-AZ Postgres for HA.

### Installation Guide
1. This templates depends on one of our `vpc-*azs.yaml` templates. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/vpc/vpc-2azs.yaml)
1. This templates depends on the `client-sg.yaml` template. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=client-sg&param_ParentVPCStack=vpc-2azs&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/client-sg.yaml)
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=rds-postgres&param_ParentVPCStack=vpc-2azs&param_ParentClientStack=client-sg&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/rds-postgres.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**

### Dependencies
* `vpc/vpc-*azs.yaml` (**required**)
* `state/client-sg.yaml` (**required**)
* `vpc/zone-*.yaml`
* `vpc/vpc-*-bastion.yaml`
* `operations/alert.yaml` (recommended)

### Limitations
* No auto scaling

## `state/s3` - S3

S3 bucket with optional public read access.

### Installation Guide
1. [![Launch Stack](./img/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=s3-bucket&templateURL=https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/{{ version }}/state/s3.yaml)
1. Click **Next** to proceed with the next step of the wizard.
1. Specify a name and all parameters for the stack.
1. Click **Next** to proceed with the next step of the wizard.
1. Click **Next** to skip the **Options** step of the wizard.
1. Check the **I acknowledge that this template might cause AWS CloudFormation to create IAM resources.** checkbox.
1. Click **Create** to start the creation of the stack.
1. Wait until the stack reaches the state **CREATE_COMPLETE**
