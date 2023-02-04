* Deploy Infrastructure in EU-WEST-1

- systems-tf-cpw-vpc
Use `improvement/SIS-141-pritunl-updates` branch and `terraform-state/aws/maicondev/eu-west-1/systems-tf-cpw-vpc/terragrunt.hcl` file to deploy up to date `vpc`.

- systems-tf-cpw-secrets
Before `terrafrunt apply` command. Download `dev-secrets.yml` file into your root folder (the same folder as where `config.yaml` is)

- systems-tf-cpw-queues
If you encountered this:

```text
var.snapshot_copy_version
  Container image version for chillibean/chillipharmdb-snapshot-copy
  Enter a value:
```

You need to use up to date `terragrunt.hcl` which is in `karoldev` environment.

- systems-tf-cpw-eks-cluster
For EKS module use `improvement/SIS-126-mailu-for-eks` branch `maicondev` environment.