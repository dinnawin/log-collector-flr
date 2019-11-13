0 创建命名空间  flr 

kubectl create ns  flr 

1 创建集群角色绑定

kubectl apply -f flr-rbac.yaml 


2 创建filebeat的配置文件 

kubectl create cm flr-filebeat-config --from-file=./filebeat.yaml  --namespace=flr

或者

kubectl apply -f flr-filebeat-configmap.yaml 


3 部署flr-redis 服务

kubectl apply -f flr-redis.yaml 

4 部署flr-filebeat.yaml  

kubectl apply -f flr-filebeat.yaml 



5 部署flr-logstash服务



kubectl create cm flr-logstash-config --from-file=./logstash.conf  --namespace=flr

或者

kubectl apply -f flr-logstash-configmap.yaml
kubectl apply -f flr-logstashyml-configmap.yaml


kubectl apply -f flr-logstash.yaml











------------------

input { stdin { } }
filter {
#ruby {
#    code => "event.set('timestamp', event.get('@timestamp') + 8*60*60)" 
#    code => "event.set('aaa', event.get('@timestamp').time.localtime)"
#    code => "event.set('bbb', event.timestamp.time.localtime + 8*60*60)"
#    code => "event.set('ccc', event.timestamp.time.localtime.strftime('%Y-%m-%d'))"
#    code => "event.set('eee', event.timestamp.time.localtime.strftime('%Y-%m-%d %H:%M:%S'))"
#}
#ruby { 
#    code => "event.set('timestamp', (event.get('@timestamp').time.localtime + 8*60*60).strftime('%Y.%m.%d'))"    
#}

#ruby { code => "event.set('bbb', event.timestamp.time.localtime + 8*60*60)" }
#ruby { code => "event.set('ccc', event.timestamp.time.localtime.strftime('%Y-%m-%d'))" }
#ruby { code => "event.set('eee', event.timestamp.time.localtime.strftime('%Y-%m-%d %H:%M:%S'))" }

ruby { 
code => "event.set('timestamp', event.get('@timestamp').time.localtime + 8*60*60)" 
}

ruby{
code => "event.set('day', (event.get('@timestamp').time.localtime + 8*60*60).strftime('%Y.%m.%d'))"
}
}
output {
stdout { codec=> rubydebug }
}
------------------

