# ansible-prometheus-k8s
k8s 部署 prometheus ansible 脚本

一、请修改 inventories/hosts 中的内容，prometheus 为执行 kubectl 的节点，prometheus-data 为安装 prometheus 和 grafana并持久化数据的节点

二、使用此 role 请确认需要修改以下几点

1. 确认 inventories/group_vars/prometheus.yml 中的 datadir  和  inventories/group_vars/all.yml 中的 datapath 一致

2. 确认 k8s 环境中各 node 的 kubernetes.io/hostname 标签为主机名，可用如下命令查看  kubectl get node --show-labels
   如果为主机名，请使用  kubernetes.io/hostname: {{ groups["prometheus-data"][0] }}
   否则，请使用 kubernetes.io/hostname: {{ prometheus.nodeselector }}，并修改 inventories/group_vars/prometheus.yml 中的 nodeselector 变量为 kubernetes.io/hostname 的值

三、执行：ansible-playbook -i inventories/hosts site.yml --extra-vars '{"adhoc_hosts":"prometheus","adhoc_remote_user":"[YOUR_USER_NAME]","adhoc_become_user":"root"}'

