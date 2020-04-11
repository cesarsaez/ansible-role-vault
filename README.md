# Ansible role for Vault with Internal Storage

The following Ansible role is intended for deploying a Vault cluster with the following assumptions:

* Vault >= 1.4 RC
* Storage backend using Integrated Storage, the [RAFT backed](https://www.vaultproject.io/docs/configuration/storage/raft/) 
* No dependecy on Consul at all, just Vault
* Vault data is stored into the VM disk from a defined path. Keep at least 1 node avaible to avoid data loss.
* A given servername  will be the cluster leader, the rest of nodes will join the RAFT cluster
* TLS certificates must be provided in advance, we can use this Ansible role to meet requirements https://github.com/cesarsaez/ansible-role-opensslcaandcerts

## Role tasks

* Disables FirewallD
* Storage path creation and permissions
* User accounts
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

NEXT ITERATION:

* auto-unseal with azure key vault
* init with PGP
* bootstrapping of ACL and backends

## Usage

```
- hosts: vault
  become: yes

  roles:
    - role: ansible-role-vault-is
      vault_root_dir: /data
      vault_bin_dir: /bin
      vault_user: vault
      vault_inital_leader_node: vault01
      vault_version: 1.4.0
      vault_download_base: https://releases.hashicorp.com/vault/
      # Vault certificate must contain certificate chain due Vault CLI limitation
      vault_cert_path: /etc/pki/tls/certs/vaultclus.example.net_chain.crt
      vault_cert_key_path: /etc/pki/tls/private/vaultclus.example.net.key
      vault_ca_path: /etc/pki/tls/certs/VAULT-CA.crt
      vault_addr: "https://{{ ansible_hostname }}:8200"
```