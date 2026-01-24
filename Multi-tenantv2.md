# Before starting configuration, please read the readme file.
## Create Agent groups
Step-1: Click the Dashboard Menu -> Agents Management -> Groups -> Add new group
```shell
  client_a
```
repeat steps for each client
Step-2: Click the Dashboard Menu -> Agents Management -> Groups -> client_a -> Files -> Edit the "agent.conf" file -> Save
```shell
  <agent_config>
    <labels>
      <label key="group">client_a</label>
    </labels>
  </agent_config>
```
repeat steps for each client
## Add agents to group
Step-1: Click the Dashboard Menu -> Agents Management -> Groups -> "client_a" -> Manage agents -> 

Step-2: Select needed Available agents -> Click Add selected items -> Apply changes

Check commands:
```shell
sudo /var/ossec/bin/agent_groups -l
```
```shell
sudo /var/ossec/bin/agent_control -l
```
## Step-1: Enable multi-tenant function
In CLI open below file
```shell
sudo nano /etc/wazuh-dashboard/opensearch_dashboards.yml
```
xxx
```shell
opensearch_security.multitenancy.enabled: true
opensearch_security.multitenancy.tenants.preferred: ["Global", "Private"]
```
Reset the wazuh-dashboard service
```shell
sudo systemctl restart wazuh-dashboard
```
## Step-2: Create tenants
Click the Dashboard Menu -> Indexer management -> Security -> Tenants -> Create tenant ->
```shell
tenant_client_a
```
repeat steps for each client
## Step-3: Create internal users
Click the Dashboard Menu -> Indexer management -> Security -> Internal users -> Create Internal user -> 
```shell
user_client_a
```
repeat steps for each client
## Step-4: Create tenant role to merge user with tenant
Click the Dashboard Menu -> Indexer management -> Security -> Roles -> Create Role ->

Name
```shell
role_tenant_a
```
Index permissions
```shell
wazuh-*
```
Document level security (group-lable-name)
```shell
{ 
    "bool": { 
        "must": { 
            "match": { 
                "agent.labels.group": "client_a" 
            } 
        } 
    } 
}
```
Tenant permissions
```shell
tenant_client_a
```
```shell
read and write
```
## Step-5: Create RBAC policy to restrict accesses 
Click the Dashboard Menu -> Server Management -> Security -> Policies -> Create Policy -> 

Policy name
```shell
client_a_policies
```
Action -> Add
```shell
agent:read
agent:reconnect
agent:restart
group:read
group:update_config
sca:read
```
Resource -> Add
```shell
agent:group
```
```shell
group:id
```
Resource identifier (group-name)
```shell
client_a
```
```shell
client_a
```
Select an effect
```shell
Allow
```








