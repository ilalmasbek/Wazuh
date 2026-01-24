# Before starting configuration, please read the readme file.
## SRV1 | Step 1: Configure the first network settings.
```shell
nano /etc/network/interfaces
```
Create Agent groups
Menu -> Agents Management -> Groups -> Add new group
  client_a
  client_B

Add agents to group
Menu -> Agents Management -> Groups -> "group-name" -> Manage agents -> select needed Available agents -> Click Add selected items -> Apply changes

CLI: sudo /var/ossec/bin/agent_groups -l
CLI: sudo /var/ossec/bin/agent_control -l


