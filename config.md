Quick Start
-----------

## 本地部署流程
- 配置ssh
node1
vim inventory/mycluster/hosts.ini

```bash
swapoff -a # 关闭分区
pip install -U pip
pip install zypper
ssh-keygen && cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

node2 & node3

```bash
swapoff -a # 关闭分区
ssh-keygen && cat <node1>/id_rsa.pub >> /root/.ssh/authorized_keys
```

node1上测试

```bash
mkdir log && ansible all -i inventory/mycluster/hosts.ini -m ping
ssh root@localhost
ssh root@<node2>
ssh root@<node3>
```

- vim roles/docker/defaults/main.yml
```bash
# 清除无用apt
```

- vim inventory/mycluster/group_vars/k8s-cluster.yml
```bash
22 kube_version: v1.9.7 # 修改kubernetes版本
```

- vim inventory/mycluster/group_vars/all.yml

```bash
# 修改docker代理
101 docker_http_proxy: "http://pc01.grepcode.cn:5080"
102 docker_https_proxy: "socks5://pc01.grepcode.cn:5080"
```

- vim roles/docker/tasks/main.yml
```bash
  1 ---                                                
  2 - name: Debug
  3   debug:
  4     msg: "{{ ansible_os_family|lower }}.yml"

```

- vim plays/cluster.yml

```bash
3 gather_facts: true
```

- install

```bash
ansible-playbook -vvv -i inventory/mycluster/hosts.ini plays/cluster.yml
ansible-playbook -vvv -i inventory/mycluster/hosts.ini plays/cluster.yml --limit @/home/zhangjin/kubespray/plays/cluster.retry
```