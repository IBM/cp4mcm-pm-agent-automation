# Ansible Playbooks for ITM

The Anisble playbooks assist with administrative tasks for ITM. The playbooks can be used in Ansible Tower in IBM Cloud Pak for Multicloud Management.

# Requirements
- Ansible Tower >=3.6.1
- ITM Server
- ITM agents host: [AMD64]


# Use Cases

## ITM agents connect to ICAM

The project transforms ITM agents to connect to ICAM, also can reverse connection from ICAM, or keep connection in dual mode both for ITM and ICAM.

### Resource

- itm2icam.yml

### Prerequisite

- ibm-cloud-apm-v6-configpack for tenant configpack file downloaded  into agent host or NFS that can be acessed by agent hosts
- ICAM Server is ready for connection

### Extra Environment Vairable

- **configpack_dir** : The directory path of IBM CLOUD APM V6 agent configpack file which name is `ibm-cloud-apm-v6-configpack.tar`, the agent host must access the path directly, it can be a local path of an NFS file path.

- **agent_dir** : The directory path of ITM V6 agent installed on agent host, for example: `/opt/IBM/MSG730FP2`

- **connection_mode** : The ITM agent connection mode, one of `[icam, itm, dual]`, default is `icam`

## PD collect

[TBD]

### Resource

- pdcollect.yml

### Prerequisite

- [TBD]

### Extra Environment Vairable

- [TBD]
