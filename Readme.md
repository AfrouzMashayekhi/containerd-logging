# Aggregate ingress log
In this project we will aggregate ingress log with the response body in json format by using filbeat as an aggregator and logstash as forwarder to elasticsearch 

all the resources are in `logging` namespace
# Adding log Format to ingress

add below variables to `nginx-configuration` configmap

log-format-upstream
 ```{ "@timestamp":"$time_iso8601", "remoteaddress":"$remote_addr", "method":"$request_method", "rurl":"$scheme://$http_host$request_uri", "protocol":"$server_protocol", "status":"$status", "bodybytes":"$body_bytes_sent", "agentname":"$http_user_agent", "xff":"$http_x_forwarded_for", "responsebody":"$request_body" }```

log-format-escape-json
```"false"```


# Filbeat DaemonSet
[Filebeat Daemonset ](./filebeat-src.yaml)

[Configmap](./filebeat_configmap.yaml)

Note: change ingress path in filebeat configmap based on your filename
# Logstash
[Logstash Deployment](./logstash.yaml)

[configmap](./logstash-configmap.yaml)

# Elastic 
``` 
helm install elastic -n logging -f elastic-values.yaml elastic/elasticsearch
```

# Kibana
```
 helm install kibana -n logging -f kibana-values.yaml elastic/kibana
```
# Reference:
https://github.com/miguelcallejasp/logging-filebeat-containerd
