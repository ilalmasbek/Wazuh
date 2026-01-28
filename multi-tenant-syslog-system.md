sudo nano /var/ossec/etc/ossec.conf
<remote>
  <connection>syslog</connection>
  <port>5514</port>
  <protocol>udp</protocol>
  <allowed-ips>0.0.0.0/0</allowed-ips>
</remote>

sudo systemctl restart wazuh-manager

sudo ss -lunp | grep 5514

sudo tcpdump -n -i any udp port 5514 -c 10






