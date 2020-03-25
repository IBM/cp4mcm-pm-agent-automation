# Ansible Playbooks for ICAM

The Anisble playbooks assist with administrative tasks for ICAM. The playbooks can be used in Ansible Tower in IBM Cloud Pak for Multicloud Management.

# Requirements
- Ansible Tower >=3.6.1
- CP4MCM ICAM Server
- ICAM agents host: [AMD64]


# Use Cases

## Download configpack

The project downloads configpack from ICAM server to target hosts.

### Resource

- `download_configpack.yml`

### Prerequisite

- Ansible Tower and CP4MCM ICAM are installed in the same cluster.

- Service account `awx` of Ansible Tower is added into cluster admin role.
  Please refer to below section of create cluster admin role binding for Ansible Tower service account.

### Extra Environment Vairable

- **account_id** : Account ID for tenant

- **configpack_type** : The configpack type supported by ICAM
  - **cnapmv6**: config pack type for `ibm-cloud-apm-v6-configpack`
  - **cnapmv8**: config pack type for `ibm-cloud-apm-v8-configpack`
  - **cnapmagents**: config pack type for `ibm-cloud-app-datacenter-agents-configpack`
  - **cnapmdc**: config pack type for `ibm-cloud-apm-dc-configpack`

- **target_dir** : The directory path configpack file on target agent host.

### Create cluster admin role binding for Ansible Tower service account

By default the service account name of Ansible Tower is `awx` in Openshift cluster, refer to Ansible Tower 3.6.1, there are two optional methods to cluster admin role binding for it.

#### Option 1: Create cluster admin role binding via Openshift web console

1. Login into Openshift web console with the admin username and password
2. Navigate to "Administration" -> "Role Binding"
3. Select Project of Ansible tower, by default it is `tower`
4. Click "Create Binding" button
5. Input the Role Binding parameters
   - Binding Type : Cluster-wide Role Binding (ClusterRoleBinding)
   - Role Binding : Set a unified name
   - Role Name : cluster-admin
   - Subject : Service Account
      - Subject Namespace : The Ansible Tower namespace, by default is tower.
      - Subject Name : The service account name, by default is awx

#### Option 2: Create cluster admin role binding via CLI

1. Login into openshift via CLI
```
oc login --token=<YOUR_TOKEN> --server=<YOUR_SERVER_URL>
```
2. Change to anible tower project, by default is `tower`
```
oc project tower
```
3. Create and edit the yaml file
```
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: awx-clusteradmin
subjects:
  - kind: ServiceAccount
    name: awx  # awx is the default value.
    namespace: <YOUR_ANSIBLE_TOWER_NAMESPACE> # default is tower
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin

```
4. Apply the yaml via
```
oc apply -f <THE_YAML_FILE_NAME>
```