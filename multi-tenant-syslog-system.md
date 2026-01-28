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

sudo ss -lunp | grep 5514

sudo tcpdump -n -i any udp port 5514 -c 10






