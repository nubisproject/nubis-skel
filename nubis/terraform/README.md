# Working with Terraform

## Set Up

Before you deploy with Terraform you need to set up your terraform.tfvars file.
There is an example copy called terraform.tfvars-dist that you can copy and
edit. It should look something like this:

```bash

account = "nubis-lab"
region  = "us-west-2"
arena = "core"
environment = "stage"
service_name = "skel-<login>"
ami="ami-XXXX"

```

### account

This is the name for the AWS account you are intending to deploy to.

### region

The AWS region you wish to deploy to, like us-east-1 or us-west-2

### arena

The arena you want to deploy into, typically this is `core`. If in doubt
just set it to `core`.

### environment

The environment is one of *sandbox*, *stage* or *prod*. For this (and all manual
deployments) you will set this to *sandbox*.

### service_name

The service_name is the name of this service. For Mozilla deployments this
should be the name of a real service as noted in [inventory](https://inventory.mozilla.org/en-US/core/service/)

### ami

You will collect this as output from nubis-builder. Once the build is complete
nubis-builder will display the ami id which you will need to manually copy into
the terraform.tfvars file. You will need to do this after every successful build.

### ssh_key_file

Path to the public ssh key file you want authorized to ssh into the launched
instances

### ssh_key_name

The account unique name you want to give to that ssh key
