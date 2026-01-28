<!-- This is a hidden comment in the rendered GitHub file.
sudo nano /var/ossec/etc/ossec.conf
<remote>
  <connection>syslog</connection>
  <port>5514</port>
  <protocol>udp</protocol>
  <allowed-ips>0.0.0.0/0</allowed-ips>
</remote>

sudo systemctl restart wazuh-manager
sudo ss -lunp | grep 5514

sudo nano /etc/filebeat/filebeat.yml
processors:
  - add_fields:
      target: fields
      fields:
        index_prefix: "wazuh-alerts-4.x-"

  # tenant B
  - add_fields:
      when:
        and:
          - equals: { agent.id: "000" }
          - network: { location: ["X.X.X.X/32"] }
      target: fields
      fields:
        index_prefix: "wazuh-alerts-4.x-client_b-"

sudo systemctl restart filebeat



-->
Soon...














<!-- default filebeat.yml config:
sudo nano /etc/filebeat/filebeat.yml

# Wazuh - Filebeat configuration file
output.elasticsearch.hosts:
        - 127.0.0.1:9200
#        - <elasticsearch_ip_node_2>:9200
#        - <elasticsearch_ip_node_3>:9200

output.elasticsearch:
  protocol: https
  username: ${username}
  password: ${password}
  ssl.certificate_authorities:
    - /etc/filebeat/certs/root-ca.pem
  ssl.certificate: "/etc/filebeat/certs/wazuh-server.pem"
  ssl.key: "/etc/filebeat/certs/wazuh-server-key.pem"
setup.template.json.enabled: true
setup.template.json.path: '/etc/filebeat/wazuh-template.json'
setup.template.json.name: 'wazuh'
setup.ilm.overwrite: true
setup.ilm.enabled: false

filebeat.modules:
  - module: wazuh
    alerts:
      enabled: true
    archives:
      enabled: false

logging.level: info
logging.to_files: true
logging.files:
  path: /var/log/filebeat
  name: filebeat
  keepfiles: 7
  permissions: 0644

logging.metrics.enabled: false

seccomp:
  default_action: allow
  syscalls:
  - action: allow
    names:
    - rseq
















DLS
{
  "bool": {
    "should": [
      { "term": { "agent.labels.group": "client_b" } },
      { "term": { "agent.id": "000" } }
    ],
    "minimum_should_match": 1
  }
}


-->
    


