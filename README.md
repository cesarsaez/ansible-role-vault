# Ansible role to deploy Vault with Internal Storage

The following Ansible role is intended for deploying a Vault cluster with the following assumptions:

* Vault >= 1.4 RC
* Storage backend using Integrated Storage, the [RAFT backed](https://www.vaultproject.io/docs/configuration/storage/raft/) (still beta at 05 April 2020)
* No dependecy on Consul at all, just Vault
* Vault data is stored into the VM disk from a defined path. Keep at least 1 node avaible to avoid data loss.
* Node1 will be the cluster leader, the rest of nodes will join the RAFT cluster

## Role tasks

* Storage path creation and permissions
* User accounts
* TLS requirements
* Vault download
* Vault systemd service
* Vault config
* Enviroment variables
* Start Vault service

TODO:

* Vault cluster init
* Vault cluster unseal
* Join nodes to RAFT cluster  https://learn.hashicorp.com/vault/operations/raft-storage#create-an-ha-cluster 
* Authentication backends
* Convert the playbook into an Ansible Role

