# nubis-skel

[![Version](https://img.shields.io/github/release/nubisproject/nubis-skel.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-skel/releases)
[![Build Status](https://img.shields.io/travis/nubisproject/nubis-skel/master.svg?maxAge=2592000)](https://travis-ci.org/nubisproject/nubis-skel)
[![Issues](https://img.shields.io/github/issues/nubisproject/nubis-skel.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-skel/issues)

This is a skeleton repository that can be used for testing or as a quick way for you to incorporate nubis into your own project.

## Prerequisites
If you are new to the Nubisproject you will need to set up some [prerequisites](https://github.com/Nubisproject/nubis-docs/blob/master/PREREQUISITES.md).

## Get the code
Next grab the latest [release](https://github.com/Nubisproject/nubis-skel/releases), extract it and copy the *nubis* directory into your code base.

## Build the project
This step is only necessary if you have changes, otherwise you can simply configure the deployment using an ami id from the following list:

|  Region   |    Ubuntu    | Amazon Linux |
|-----------|--------------|--------------|
| us-east-1 | ami-e4eaa584 | ami-987e078f |
| us-west-2 | ami-e963be89 | ami-2563be45 |

If you run *nubis-builder* it will output an ami id for you to use.
```bash
nubis-builder build
```

## Configure the deployment
Create a nubis/terraform/terraform.tfvars file by copying the [terraform.tfvars-dist](nubis/terraform/terraform.tfvars) file and editing the parameter values. More detailed instructions can be found [here](nubis/terraform/README.md#set-up).
```bash
cp nubis/terraform/terraform.tfvars-dist nubis/terraform/terraform.tfvars
vi nubis/terraform/terraform.tfvars
```

## Deploy the application
You are now ready to deploy your application. Be sure to replace "\<username\>" with a unique application name. You can find more detailed instructions [here](nubis/terraform/README.md#commands-to-work-with-terraform).
```bash
$> cd nubis/terraform
$> terraform get -update=true
$> terraform plan
$> terraform apply
```

## Help
If you run into any issues, feel free to reach out to us. We hang out in #nubis-users on irc.mozilla.org.
