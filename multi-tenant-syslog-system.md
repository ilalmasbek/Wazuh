## Step-1: Edit the wazuh manager (ossec) file to accept syslogs.
Open the file
```shell
sudo nano /var/ossec/etc/ossec.conf
```
Add the piece command
```shell
<remote>
  <connection>syslog</connection>
  <port>5514</port>
  <protocol>udp</protocol>
  <allowed-ips>0.0.0.0/0</allowed-ips>
</remote>
```
Restart the wazuh manager service to apply the added command
```shell
sudo systemctl restart wazuh-manager
```
## Step-2: Configuring pipeline to redirect incoming syslogs to the desired index patterns via srcip addresses
Open the pipeline file
```shell
sudo nano /usr/share/filebeat/module/wazuh/alerts/ingest/pipeline.json
```
Make sure the index_name_prefix line is like this
```shell
"index_name_prefix": "{{fields.index_prefix}}",
```
Add the set commands
```shell
    {
      ="set": {
        "field": "fields.index_prefix",
        "value": "wazuh-alerts-4.x-"
      }
    },
    {
      "set": {
        "field": "fields.index_prefix",
        "value": "client_b-wazuh-alerts-4.x-",
        "if": "ctx.location != null && ctx.location == '206.62.53.82'"
      }
    },
```
Finally, make sure the command looks like this.

<img width="787" height="499" alt="image" src="https://github.com/user-attachments/assets/3935eb4b-1e7e-46e0-9c23-ec3a9d69a664" />

## Step-3: Configure network devices so that they sends syslog to wazuh server public ip address or domain

## Step-4: Сheck whether syslogs are being sent to the Wazuh server.
Wrute these commands in the command line
```shell
sudo ss -lunp | grep 5514
```
<img width="1185" height="63" alt="image" src="https://github.com/user-attachments/assets/57d873c8-039c-43f9-b9cd-369f9241252c" />

```shell
sudo tcpdump -n -i any udp port 5514 -c 10
```
<img width="902" height="346" alt="image" src="https://github.com/user-attachments/assets/04202029-d55c-48cf-a417-717577c46fac" />

## Step-5: Create an Index Pattern as we specified in the pipeline config
Open the Wazuh Dashboard via Global tenant -> Clieck the Menu -> Dashboard Management -> Dashboard Managements -> Index Patterns -> Create index pattern
<img width="1775" height="829" alt="image" src="https://github.com/user-attachments/assets/c59b1d5a-40ec-42d2-af40-e8b5f20e209d" />
Fill the Index pattern name as in the pipeline config-> After that, click the "Next Step"
<img width="1368" height="658" alt="image" src="https://github.com/user-attachments/assets/94373f93-1033-4555-8a80-79ce982a76ac" />
In the Time field, specify @timestamp -> After, just click the "Create index pattern" 
<img width="1359" height="602" alt="image" src="https://github.com/user-attachments/assets/31f63637-7c3a-4eae-95f7-61c4a2c5d828" />
## Step-6: Add created Index Pattern to Tenant role
Open the Manu -> Indexer Management -> Security -> Roles -> 

Search needed role "role_tenant_client_b"
<img width="1667" height="414" alt="image" src="https://github.com/user-attachments/assets/e3f40777-17b0-4749-9fe1-9fe27337ff3a" />
Select the role -> click the Actions -> Edit
<img width="1678" height="481" alt="image" src="https://github.com/user-attachments/assets/b0d7515b-0217-4b78-bbb1-9c51ca5369c0" />












