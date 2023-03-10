# Azure private OpenShift cluster
This repo contains playbooks to automate the setup of an Azure subscription in order to install a private OpenShift cluster.

## Usage
1. Install [Azure.Azcollection](https://galaxy.ansible.com/azure/azcollection), pay attention to python library requirements after collection install.
2. Create an environment with Azure login informations:
    ```bash
    export AZURE_SUBSCRIPTION_ID="redacted"
    export AZURE_CLIENT_ID="redacted"
    export AZURE_SECRET="redacted"
    export AZURE_TENANT="redacted"
    ```
    1. Alternatively, login with Azure CLI, ansible will reuse the credentials obtained.
3. Configure [`vars.yml`](ansible/vars.yml) variable file
4. Run [`setup.ansible.yml`](ansible/setup.ansible.yml) playbook

The playbook will create
- **one** resource group
- **one** vnet
- **two** private subnets, one for masters node, one for worker nodes
- **one** (optional) Nat gateway that will be linked to master and worker subnets
- **two** security groups, one for the private master and worker subnets, one for the public subnet to allow to ssh to bastion host

_Without Nat Gateway, master and worker subnets will not have direct internet access_, a proxy can be added to the bastion host to simulate this use case as well.

## Teardown
A playbook is provided to teardown all the objects created: [`destroy.ansible.yml`](ansible/destroy.ansible.yml).

## OpenShift installation
A sample [`install-config.yaml`](ocp-install-config/install-config.yaml) is provided as a reference.