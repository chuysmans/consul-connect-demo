# Consul Connect Demo

An incredibly modified version of <https://github.com/norhe/cc_demo>

This repo demonstrates using Consul Connect.  There are a number of moving parts.  

First, a Consul cluster is deployed.  Then, a set of client nodes are deployed.  All client nodes have Consul running.

There are four pieces of the application.  A mongo database to store records, a set of APIs called Product and Listing, and a web client that renders results.  All pieces communicate with one another using the built in Consul Connect proxies.

## Packer

The packer configuration used to build the machine images is in the `packer` directory. All images
except the one built with Vault Enterprise are currently public. Use `make` if you want to build the
AWS AMIs

## Terraform

### AWS

You will need AWS credentials, set your credentials appropriately <https://www.terraform.io/docs/providers/aws/index.html>

- Change to the `terraform/aws` directory
- Copy `terraform.auto.tfvars.example` to `terraform.auto.tfvars` and update appropriately
  - `mode             = "noconnect"` deploys with Consul service discovery but _without_ Connect
  - `mode             = "connect"` deploys with Consul service discovery _and_ Connect
- Do the normal `terraform init`, `terraform plan`, `terraform apply` dance

*Note*: It takes a couple minutes for everything to spin up and be reachable after Terraform is done, until then the `web_client` will show some errors connecting to backend services. Just wait. 

steps:
1. describe the demo setup, a 3 tier application with web/api/db tiers.
2. open dc1 webclient
3. open dc2 webclient, talk about the difference in contents.
4. open consul UI, do normal service discovery/registration/connect talks.
5. login into one listing API box run steps in home dir.
6. login into one webclient box and run steps in home dir.
7. change of KV product/run to false to stop product API, and see the automagic failover happens.
8. create intentions to deny everything, see the webapp fail, then add back one by one to see the effect.

