# ansible-prometheus-k8s
k8s 部署 prometheus ansible 脚本，
使用次脚本的前提是您已经部署好 k8s 环境和 ingress，并且可访问外网，如不可访问外网可以提前下载好 image，并修改inventories/group_vars/prometheus.yml 中的 repo 本地化安装

 镜像名称：
 
 ```
 xuejipeng/tiny-tools:latest         
 xuejipeng/kube-state-metrics:v1.2.0           
 xuejipeng/alertmanager:v0.12.0         
 xuejipeng/grafana:4.6.3          
 xuejipeng/node-exporter:v0.15.2       
 xuejipeng/prometheus:v2.0.0   
 ```    
一、请修改 inventories/hosts 中的内容，prometheus 为执行 kubectl 的节点，prometheus-data 为安装 prometheus 和 grafana 并持久化数据的节点

二、使用此 role 请确认需要修改以下几点

1. 确认 inventories/group_vars/prometheus.yml 中的 datadir  和  inventories/group_vars/all.yml 中的 datapath 一致

2. 确认 k8s 环境中各 node 的 kubernetes.io/hostname 标签 

   kubectl get node --show-labels

   请使用 kubernetes.io/hostname: {{ prometheus.nodeselector }}，并修改 inventories/group_vars/prometheus.yml 中的 nodeselector 变量为 kubernetes.io/hostname 的值

三、执行：ansible-playbook -i inventories/hosts site.yml --extra-vars '{"adhoc_hosts":"prometheus","adhoc_remote_user":"[YOUR_USER_NAME]","adhoc_become_user":"root"}'

