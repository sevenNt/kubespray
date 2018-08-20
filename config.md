Quick Start
-----------

- 1. 配置ssh
node1 
```bash
swapoff -a # 关闭分区
pip install -U pip
pip install zypper
ssh-keygen && cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

node2
```bash
swapoff -a # 关闭分区
ssh-keygen && cat <node1>/id_rsa.pub >> /root/.ssh/authorized_keys
```

node1上测试
```bash
ssh root@localhost
ssh root@node2
```

- 2. vim roles/docker/defaults/main.yml

- 3. vim inventory/mycluster/group_vars/all.yml
```bash
101 docker_http_proxy: "http://pc01.grepcode.cn:5080"
102 docker_https_proxy: "socks5://pc01.grepcode.cn:5080"
```
- 4. mkdir log && ansible all -i inventory/mycluster/hosts.ini -m ping

- 5. vim roles/docker/tasks/main.yml
```shell
  1 ---                                                                                                                                                                                                                                                              
  2 - name: Debug
  3   debug:
  4     msg: "{{ ansible_os_family|lower }}.yml"

```

- 5. vim plays/cluster.yml
```bash
3 gather_facts: true
```

- 6. install
```shell
ansible-playbook -vvv -i inventory/mycluster/hosts.ini plays/cluster.yml
ansible-playbook -vvv -i inventory/mycluster/hosts.ini plays/cluster.yml --limit @/home/zhangjin/kubespray/plays/cluster.retry
```