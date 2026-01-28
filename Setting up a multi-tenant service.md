<img width="1365" height="771" alt="image" src="https://github.com/user-attachments/assets/18dc2326-f1d2-47be-aca2-11d4cc8001b8" />

## Step-1: Create Agent groups
Click the Dashboard Menu -> Agents Management -> Groups -> Add new group
```shell
  client_a
```
Node: repeat steps for each client

Click the Dashboard Menu -> Agents Management -> Groups -> client_a -> Files -> Edit the "agent.conf" file -> Save
```shell
  <agent_config>
    <labels>
      <label key="group">client_a</label>
    </labels>
  </agent_config>
```
Node: repeat steps for each client
## Step-2: Add agents to group
Click the Dashboard Menu -> Agents Management -> Groups -> "client_a" -> Manage agents -> 
<img width="1882" height="149" alt="image" src="https://github.com/user-attachments/assets/6ef87ae6-92ea-4503-8a27-01f3532e12d8" />

Select needed Available agents -> Click Add selected items -> Apply changes
<img width="1882" height="674" alt="image" src="https://github.com/user-attachments/assets/cfed411f-2fc4-4f7a-b130-db3314e749d4" />

Check commands:
```shell
sudo /var/ossec/bin/agent_groups -l
```
```shell
sudo /var/ossec/bin/agent_control -l
```
## Step-3: Enable multi-tenant function
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
## Step-4: Create tenants
Click the Dashboard Menu -> Indexer management -> Security -> Tenants -> Create tenant ->
```shell
tenant_client_a
```
Node: repeat steps for each client
## Step-5: Create internal users
Click the Dashboard Menu -> Indexer management -> Security -> Internal users -> Create Internal user -> 
```shell
user_client_a
```
Node: repeat steps for each client
## Step-6: Create tenant role to merge user with tenant
Click the Dashboard Menu -> Indexer management -> Security -> Roles -> Create Role ->

Name
```shell
role_tenant_a
```
Index
```shell
wazuh-*
```
Index permissions
```shell
read, search, indeces_monitor
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
<img width="1871" height="791" alt="image" src="https://github.com/user-attachments/assets/a54ce21c-0d10-4de5-89fa-b02e262fb2e0" />

Tenant permissions
```shell
tenant_client_a
```
```shell
read and write
```
<img width="1440" height="235" alt="image" src="https://github.com/user-attachments/assets/b327a1ef-1272-43e4-88fe-0092a0503a09" />
## Step-7: Create Mapping users to marge Tenant Role with Internal User
Click the Dashboard Menu -> Indexer management -> Security -> Roles -> role_tenant_client_a -> Mapped users -> Map users
<img width="1897" height="630" alt="image" src="https://github.com/user-attachments/assets/f456b85e-1f94-4cfc-8736-7ec446ab5983" />
Users: user_client_a

Click the Map
<img width="1900" height="679" alt="image" src="https://github.com/user-attachments/assets/a50bc515-d643-4f6a-9e39-86739f3e8794" />

## Step-8: Create RBAC policy to restrict accesses 
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
<img width="924" height="557" alt="image" src="https://github.com/user-attachments/assets/55e59bcd-9e41-4674-b8ab-bf5da3afe24f" />

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
<img width="927" height="380" alt="image" src="https://github.com/user-attachments/assets/f36a88af-6284-4952-8e3a-2d94781be90e" />

Node: repeat steps for each client
## Step-9: Create role to add RBAC policy 
Click the Dashboard Menu -> Server Management -> Security -> Roles -> Create Role -> 

Role name
```shell
client_a_role
```
Policies
```shell
client_a_policies
```
<img width="537" height="412" alt="image" src="https://github.com/user-attachments/assets/3579329a-66af-4355-b37a-6f61ac8b8747" />

Node: repeat steps for each client
## Step-10: Create Mapping role to marge RBAC Role with Internal User
Click the Dashboard Menu -> Server Management -> Security -> Roles mapping -> Create Role mapping -> Save role mapping

Role name
```shell
map_client_a_role
```
Roles
```shell
client_a_role
```
Map internal users
```shell
user_client_a
```
<img width="917" height="823" alt="image" src="https://github.com/user-attachments/assets/c3b733b3-7ccd-4439-b4ac-2bc1985523f1" />

Node: repeat steps for each client
## Step-11: Change run_as status of default group 
Open the wazuh.yml file through CLI
```shell
sudo nano /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml
```
Edit run_as status to true
```shell
hosts:
  - default:
      url: https://127.0.0.1
      port: 55000
      username: wazuh-wui
      password: "xxx"
      run_as: true
```
Restart the dashboard service
```shell
sudo systemctl restart wazuh-dashboard
```
