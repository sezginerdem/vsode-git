# Deployment Instructions

## Prerequisites

Ensure that brew, tfenv and tgenv are installed to  the local machine.
View documentation for installation instructions.

* https://brew.sh/
* https://github.com/tfutils/tfenv (terraform on v1.1.3)
* https://github.com/cunymatthieu/tgenv (terragrunt should be set to latest version)


## Configure Development Environment

In order to configure & build the chillipharm infrastructure
terragrunt allows us to define variables globally using a yaml
configuration that is included and merged using the
terraform built in functions available to
terragrunt. Edit the `config.yaml` specific to the enviroment as
this will be our source of truth for deployments and important
values moving forward. The example belows shows how you must
configure keys and value pairs inside the `config.yaml` file.

```
account_num: "<aws_account_num>"
aws_region: "eu-west-2"
aws_regions: ['eu-west-1','eu-central-1']
envlabel: "<envlabel>"
domain: "<envlabel>.chillipharm-dev.com"
netdomain: "<envlabel>.chillipharm-dev.net"
private_hosted_zone: "<envlabel>".chillipharm.private"
cidr: "<cidr_block>"
public_subnets: "<[public_subnet, public_subnet, public_subnet]>"
private_subnets: "<[private_subnet, private_subnet, private_subnet]>"
```
Once you are done editing the values in config.yaml save the file.

Please ask systems before inputting the following values, as these values below must not overlap with other development enviroments.

* <cidr_block>
* <public_subnets>
* <private_subnets>

### Configure the terragrunt root HCL files

Before we run any modules using terragrunt, we need to ensure
we have a root `terragrunt.hcl` file under each of the following
folders `eu-west-2`, `eu-west-1` & `eu-central-1`, the reason being
we need to keep our terraform modules DRY. The AWS provider
and backend configurations are included in each of these root
`terragrunt.hcl` files for each region.

Inside each of these terragrunt.hcl files amend the following
locals specific to your development environment. You will only
need to edit two values inside the files.

```
envlabel = <envlabel>
account_num = <aws_account_number>
```
Once you have done editing each of the root `terragrunt.hcl`
files save them.

### Configure the tf_bootstrap root HCL file

To deploy infastrucuture to AWS via terragrunt,
we need to ensure we have a root `tf_bootstrap.hcl` file.
Configuring this file allows IAM permissions for
modules to be deployed via terragrunt. This file
is found under the `eu-west-1` directory.

Inside the `tf_bootstrap.hcl` file amend the following
locals specific to your development environment. You will only
need to edit two values inside the files.

```
envlabel = <envlabel>
account_num = <aws_account_number>
```
Once you have done editing each the root `tf_bootstrap.hcl`
file save it.

### Configure dev-secrets.yaml files
To deploy chillipharm specfic secrets to AWS Secrets Manager , we need to ensure we have a `dev-secrets.yaml`
file deployed. You will need copy the `dev-secrets.yaml` from 1Password into the root of the AWS folder.

### Configure AWS Simple Mail Service (only prod/uat & staging envs)
On the AWS web user interface we need to create credentails that allow authentication with Amazon SES, this
allows chillipharm applications to send mail through the Amazon SES interface. The following instructions
need to be preformed in both `eu-west-1` and `eu-central-1`.

* Locate AWS Simple Mail Service (on webconsole)
* Create SMTP credentails and note somewhere safe (will be needed for next section)
* Create a verified identity for email address `systems@chillibean.com`
* Check `systems@chillibean.com` inbox and verify identity

### Configure ses-secrets.yaml
The `ses-secrets.yaml` is the set of SMTP credetials generated in the section above. You will need
to copy `ses-secrets.yaml` from 1Password in the root of the AWS folder.

Note: If you are deploying on development enviroments then for all values inside the `ses-secrets.yaml` file enter `nil`

### Configure GitHub Access Token

We need to ensure we have a Github personal access token
configured to commit terraform state files
to GitHub repository
https://github.com/Chillibean/iamroot_state.


These terraform state files only contain information about the following resources

* tfbootstrap role
* Terraform State Bucket
* DynamoDB
Please read the following link to create a GitHub personal access token (classic) https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token

Once you have created the personal access token, on the local machine ensure you create the following file
```
(1) touch ~/.git.yml
(2) vim ~/.git.yml
(3) Enter the following configuration
        (3.1) access_token: "<enter_access_token>"
```
Save the file once you have completed creating the access token on the local machine


### Deploy Infrastructure in EU-WEST-2

We need to build the following resources in the `eu-west-2` region under the `_global` folder.

* tf_bootstrap Role
* Terraform State Bucket
* DynamoDB

Follow the instructions below to deploy these resources in the `eu-west-2`
region under the `_global` folder.
```
1. cd /eu-west-2/_global/terraform-backend -> terraform apply
2. cd /eu-west-2/_global/systems-tf-bootstrap-role -> terraform apply
3. cd /eu-west-2/_global/terraform-backend-commit -> terraform apply
```
Once above is deployed succesfully build the next set of resources to be
deployed in `eu-west-2` region using terragrunt.

```
4. cd /eu-west-2/systems-tf-deployment-role -> terragrunt apply
5. cd /eu-west-2/systems-tf-logging-s3-buckets -> terragrunt apply
6. Cd /eu-west-2/systems-tf-cpw-s3-buckets -> terragrunt apply
7. Cd eu-west-2/systems-tf-route53-dev-env -> terragrunt apply
```

### Deploy Infrastructure in EU-WEST-1
We need to also build the following resources in the `eu-west-1` region under the `_global` folder.

* Terraform State Bucket
* DynamoDB

Follow the instructions below to deploy these resources in the `eu-west-1`
region under the `_global` folder.

```
1. cd /eu-west-1/_global/terraform-backend -> terraform apply
2. cd /eu-west-1/_global/terraform-backend-commit -> terraform apply
```

Once above is deployed succesfully build the next set of resources to be
deployed in `eu-west-1` region using terragrunt.
- Switch to `improvement/SIS-141-pritunl-updates` branch and run `git pull`. Replace `maicondev/eu-west-1/systems-tf-cpw-vpc/terragrunt.hcl` with your's.

```
3. Cd /eu-west-1/systems-tf-cpw-vpc -> terragrunt apply
4. Cd /eu-west-1/systems-tf-cpw-local-domain -> terragrunt apply
5. Cd /eu-west-1/systems-tf-cpw-certificates -> terragrunt apply
```

```
6. Cd /eu-west-1/systems-tf-cpw-secrets -> terragrunt apply
```
- If you encountered this:
```
var.snapshot_copy_version
  Container image version for chillibean/chillipharmdb-snapshot-copy
  Enter a value:
```

- Replace `karoldev/eu-west-1/systems-tf-cpw-secrets/terragrunt.hcl` with your's.

- If you don't keep going.
```
7. Cd /eu-west-1/systems-tf-cpw-queues -> terragrunt apply
8. Cd /eu-west-1/systems-tf-cpw-db-seed-bucket -> terragrunt apply
9. Cd /eu-west-1/systems-tf-cpw-aurora-postgresql -> terragrunt apply
10.  Cd /eu-west-1/systems-tf-cpw-aurora-backup-and-moniotiring -> terragrunt apply
```

- Switch to `improvement/SIS-126-mailu-for-eks` branch and run `git pull`. Replace `maicondev/eu-west-1/systems-tf-cpw-eks-cluster/terragrunt.hcl` with your's.

```
11.   Cd /eu-west-1/systems-tf-cpw-eks-cluster -> terragrunt apply
```
