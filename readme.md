Start ELK test enviroment 


## Minikube enviroment

```kubectl apply -f kube/.```

Provision take a time 5-10 min

```kubectl  get pod```

should look like 

```
$ kubectl  get pod
NAME                        READY   STATUS    RESTARTS   AGE
es-0                        1/1     Running   0          4m1s
es-1                        1/1     Running   0          3m57s
es-2                        1/1     Running   0          3m53s
filebeat-gj9gd              1/1     Running   0          4m1s
kibana-6495df5cd7-c8m9k     1/1     Running   0          4m1s
logstash-7d559d7979-rbp97   1/1     Running   0          4m1s
logstash-7d559d7979-zzhmp   1/1     Running   0          38s
noise-app-fdf6ddb79-795r2   1/1     Running   0          4m1s
```

for test need run command and see that index logstash- present at list

```
$ kubectl  exec -ti es-0 -- curl http://elasticsearch:9200/_cat/indices | grep logstash-
green open logstash-2020.10.17-000001     g9xPTgltQ7qQTrPmsxZ2Vw 1 1 4627   0   4.2mb  2.4mb
```

to get access for kibana run this command

```
echo http://$(kubectl get node -o json | jq -r '.items[0].status.addresses[] | select( .type == "InternalIP").address'):$(kubectl  get service kibana -o json | jq '.spec.ports[0].nodePort')
```


response looks like 

```

$ echo http://$(kubectl get node -o json | jq -r '.items[0].status.addresses[] | select( .type == "InternalIP").address'):$(kubectl  get service kibana -o json | jq '.spec.ports[0].nodePort')
http://192.168.99.101:32515


```

Inside kibana need to select index 

message noise app can be look by query

```kubernetes.container.name: noise-app```
