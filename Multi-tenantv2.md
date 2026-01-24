# Before starting configuration, please read the readme file.
## Enable multi-tenant function
Step-1: In CLI open below file
```shell
sudo nano /etc/wazuh-dashboard/opensearch_dashboards.yml
```
Step-2: 
```shell
opensearch_security.multitenancy.enabled: true
opensearch_security.multitenancy.tenants.preferred: ["Global", "Private"]
```
Step-3: Reset the wazuh-dashboard service
```shell
sudo systemctl restart wazuh-dashboard
```
## Create Agent groups
Step-1: Click the Dashboard Menu -> Agents Management -> Groups -> Add new group
```shell
  client_a
```
```shell
  client_b
```
Step-2: Click the Dashboard Menu -> Agents Management -> Groups -> client_a -> Files -> Edit the "agent.conf" file -> Save
```shell
  <agent_config>
    <labels>
      <label key="group">client_a</label>
    </labels>
  </agent_config>
```

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

