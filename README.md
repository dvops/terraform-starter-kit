# Terraform-Starter-Kit

The code here bootstraps an infrastructure from a single entrypoint, the `./do` script. Firstly, Ansible creates a state bucket for Terraform, and then Terraform can be run layer-by-layer to provision any kind of infrastructure.

For Terraform, the `./do` script runs both headlessly and interactively. Interactively, the script will offer options based on the available environments, available layers, and the three Terraform commands, plan, apply and destroy.

## Setup

This starter-kit is built around AWS as a provider, and as such, requires `aws-vault` to be installed in order to manage profiles and credentials security. Access keys and secrets are not to be kept in plaintext.

AWS Vault may be found here: https://github.com/99designs/aws-vault

Once this is installed, please add credentials for your profiles as instructed.

    ~/.aws/config

    [profile account-a]
    mfa_serial=arn:aws:iam::ACCOUNT_NUMBER:mfa/USERNAME
    output=json
    region=eu-west-1

You would add credentials for this profile by doing...

    aws-vault add account-a

...and following the prompts.


#### Example

To build a bare bones infra consisting on a single VPC with a single private subnet, do the following once the dependencies are installed and the environment variables are present.

    ./do bootstrap # this creates the S3 bucket for the Terraform state
    ./do

    # And interactively choose an env, then the first layer, 001_vpc
    # Then 'plan' or 'apply'

This will plan or build the infrastructure. When finished, teardown the infrastructure.

    ./do # follow prompts to destroy
    ./do teardown

This will remove the cleanup the state bucket in S3.

#### Dependencies

    Python 2.7
    Ansible
    AWS Cli
    AWS Vault
    Boto
    Terraform
    Bash

### Using `direnv`

Copy `envrc.template` to `.envrc` and fill in the appropriate details.

### Add your own environment

Copy `./environments/999_example` to a new directory under ./environments.

## Run headlessly

#### Setup/teardown

    ./do bootstrap

And

    ./do teardown

Then, the format is:

    ./do <command> -e <env_name> -l <layer_name>

Where available commands are `plan`, `apply` and `destroy`.

    ./do plan -e 999_example -l 001_vpc
