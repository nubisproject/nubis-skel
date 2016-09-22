## Fork the cloudz repository on github

## Clone the new repository locally

```bash
$> git clone https://github.com/gozer/nubis-cloudz
$> cd nubis-cloudz
```

## Change the name of the application

```bash
$> grep skel * -rl | xargs perl -pi -e's/skel/cloudz/g
```

## Build the application

```
$> nubis-builder build

## Configure the deployment for the application

```bash
$> cd nubis/terraform
$> cat <<EOF > terraform.tfvars
account = "nubis-training-2016"
region  = "us-west-2"
environment = "stage"
service_name = "$USER-cloudz"
EOF
```

## Deploy time with terraform

### Get modules

```bash
$> aws-vault exec nubis-training-2016-admin -- terraform get -update=true
```

### Plan the deployment

```bash
$> aws-vault exec nubis-training-2016-admin -- terraform plan -var ami=ami-XXXX
```

### Apply the deployment

```bash
$> aws-vault exec nubis-training-2016-admin -- terraform apply -var ami=ami-XXXX
```

## Update the application

```bash
$> cd ../..
$> vi nubis/puppet/files/index.html
$> nubis-builder build
$> ( cd nubis/terraform && aws-vault exec nubis-training-2016-admin -- terraform plan -var ami=ami-YYY )
$> ( cd nubis/terraform && aws-vault exec nubis-training-2016-admin -- terraform apply -var ami=ami-YYY )
```

## Cleanup after yourselves

```bash
$> cd nubis/terraform
$> aws-vault exec nubis-traininig-2016-admin -- terraform plan -var ami=ami-YYY -destroy
$> aws-vault exec nubis-traininig-2016-admin -- terraform destroy
```
