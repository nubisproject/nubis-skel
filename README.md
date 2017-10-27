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

### Setup your shell

These are convenience steps to make the following commands a bit more manageable.
Be sure to replace `<login>` with your login name. You may also need to adjust
the account name and number if you are not building in the Nubis training
account.

```bash

ACCOUNT_NAME='nubis-training'; ACCOUNT_NUMBER='517826968395'; LOGIN='<login>'

alias vault-exec="aws-vault exec ${ACCOUNT_NAME}-admin --"

NUBIS_DOCKER=( 'docker' 'run' \
                '-u' "$UID:$(id -g)" \
                '--interactive' \
                '--tty' \
                '--env-file' "$(echo ~)/.docker_env" \
                '-v' "$PWD:/nubis/data" )

```

## Build the project

Run *nubis-builder* and it will output an AMI id for you to use.

```bash

NUBIS_BUILD_VERSION=$(curl -k -s -S \
https://registry.hub.docker.com/v1/repositories/nubisproject/nubis-builder/tags \
| jq --raw-output '.[]["name"]' | \
sort --field-separator=. --numeric-sort --reverse | \
grep -m 1 "^v")

vault-exec "${NUBIS_DOCKER[@]}" -e GIT_COMMIT_SHA=$(git rev-parse HEAD) \
    "nubisproject/nubis-builder:${NUBIS_BUILD_VERSION}"

```

## Configure the deployment

Create a nubis/terraform/terraform.tfvars file by copying the
[terraform.tfvars-dist](nubis/terraform/terraform.tfvars) file and editing the
parameter values. More detailed instructions can be found [here](nubis/terraform/README.md#set-up).
NOTE: Be sure to use the AMI id you got from the build step.

```bash

cp nubis/terraform/terraform.tfvars-dist nubis/terraform/terraform.tfvars
vi nubis/terraform/terraform.tfvars

```

## Deploy the application

You are now ready to deploy your application. Once you have tested, be sure to
destroy the deployment to avoid unnecessary cost in AWS.

```bash

NUBIS_DEPLOY_VERSION=$(curl -k -s -S \
https://registry.hub.docker.com/v1/repositories/nubisproject/nubis-release/tags \
| jq --raw-output '.[]["name"]' | \
sort --field-separator=. --numeric-sort --reverse | \
grep -m 1 "^v")

vault-exec "${NUBIS_DOCKER[@]}" \
    "nubisproject/nubis-deploy:${NUBIS_DEPLOY_VERSION}" plan

vault-exec "${NUBIS_DOCKER[@]}" \
    "nubisproject/nubis-deploy:${NUBIS_DEPLOY_VERSION}" apply

```

## View the site

Terraform creates a route53 hosted zone and a cname record. And the resulting
url will be part of the outputs:

```bash

Outputs:

  address = https://www.<service_name>-<env>.<env>.<region>.<account_name>.nubis.allizom.org/

```

### Login to the web instance

If you have only one EC2 instance and your ssh keys are on the jumphost, you can
login by:

```bash

ssh -A -t <login>@jumphost.<env>.core.<region>.<account-name>.nubis.allizom.org \
"ssh -A -t ubuntu@<service_name>.service.consul"

```

## Destroy the deployment

Lastly, clean up the deployment which helps reduce cost , and it is just tidy.

```bash

vault-exec "${NUBIS_DOCKER[@]}" \
    "nubisproject/nubis-deploy:${NUBIS_DEPLOY_VERSION}" destroy

```

## Help

If you run into any issues, feel free to reach out to us. We hang out
in #nubis-users on Slack.
