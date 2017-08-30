

# nubis-skel

[![Version](https://img.shields.io/github/release/nubisproject/nubis-skel.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-skel/releases)
[![Build Status](https://img.shields.io/travis/nubisproject/nubis-skel/master.svg?maxAge=2592000)](https://travis-ci.org/nubisproject/nubis-skel)
[![Issues](https://img.shields.io/github/issues/nubisproject/nubis-skel.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-skel/issues)

This is a skeleton repository that can be used for testing or as a quick way for
you to incorporate Nubis into your own project.

## Prerequisites

If you are new to the Nubisproject you will need to set up some [prerequisites](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md).

## Get the code

Next grab the latest [release](https://github.com/Nubisproject/nubis-skel/releases),
extract it and copy the *nubis* directory into your code base.

## Build the project

This step is only necessary if you have changes, otherwise you can simply
configure the deployment using an AMI id from the following list:

|  Region   |    Ubuntu    | Amazon Linux |
|-----------|--------------|--------------|
| us-east-1 | ami-e4eaa584 | ami-987e078f |
| us-west-2 | ami-e963be89 | ami-2563be45 |

If you run *nubis-builder* it will output an AMI id for you to use.

```bash

vault-exec "${NUBIS_DOCKER[@]}" -e GIT_COMMIT_SHA=$(git rev-parse HEAD) nubisproject/nubis-builder:v0.4.0

```

## Configure the deployment

Create a nubis/terraform/terraform.tfvars file by copying the
[terraform.tfvars-dist](nubis/terraform/terraform.tfvars) file and editing the
parameter values. More detailed instructions can be found [here](nubis/terraform/README.md#set-up).

```bash

cp nubis/terraform/terraform.tfvars-dist nubis/terraform/terraform.tfvars
vi nubis/terraform/terraform.tfvars

```

## Deploy the application

You are now ready to deploy your application. Once you have tested, be sure to
destroy the deployment to avoid unnecessary cost in AWS.

```bash

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.2.0 plan

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.2.0 apply

vault-exec "${NUBIS_DOCKER[@]}" nubisproject/nubis-deploy:v0.2.0 destroy

```

## Help

If you run into any issues, feel free to reach out to us. We hang out
in #nubis-users on Slack.
