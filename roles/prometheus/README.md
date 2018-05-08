使用此 role 请确认需要修改以下几点

1. 确认 inventories/aws/ci/group_vars/prometheus.yml 中的 datadir  和  inventories/aws/ci/group_vars/all.yml 中的 datapath 一致

2. 确认 k8s 环境中各 node 的 kubernetes.io/hostname 标签为主机名，可用如下命令查看  kubectl get node --show-labels
   如果为主机名，请使用  kubernetes.io/hostname: {{ groups["prometheus-data"][0] }}
   否则，请使用 kubernetes.io/hostname: {{ prometheus.nodeselector }}，并修改 inventories/aws/ci/group_vars/prometheus.yml 中的 nodeselector 变量

  
ansible-playbook -i inventories/aws/ci/hosts service.yml --extra-vars '{"adhoc_hosts":"prometheus","adhoc_remote_user":"xuejipeng","adhoc_become_user":"root"}'