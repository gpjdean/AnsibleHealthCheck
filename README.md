## Ansible主机巡检生成网页报告

## 项目地址
```bash
https://github.com/gpjdean/AnsibleHealthCheck.git
```



## 安装Ansible
```bash
yum -y install epel-release
yum -y install ansible
ansible --version
```



## 克隆代码
```bash
[root@instance-euwvmd1u ~]# git clone https://ghfast.top/https://github.com/gpjdean/AnsibleHealthCheck.git
Cloning into 'ansible-HealthCheck'...
remote: Enumerating objects: 40, done.
remote: Counting objects: 100% (40/40), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 40 (delta 12), reused 35 (delta 7), pack-reused 0 (from 0)
Unpacking objects: 100% (40/40), done.
```



## 修改项目ansible.cfg配置
```bash
# 进入到项目路径
[root@instance-euwvmd1u ~]# cd ansible-HealthCheck

# 指空文件内容
[root@instance-euwvmd1u ansible-HealthCheck]# >ansible.cfg 

# 编辑配置文件，将下面的内容写入到ansible.cfg文件内
[root@instance-euwvmd1u ansible-HealthCheck]# vim ansible.cfg

[defaults]
host_key_checking=False
remote_user=root
filter_plugins=/root/ansible-HealthCheck/filter_plugins
[inventory]
[privilege_escalation]
[paramiko_connection]
[ssh_connection]
[persistent_connection]
[accelerate]
[selinux]
[colors]
[diff]
```


## 配置hosts主机信息
```bash
[root@instance-euwvmd1u ansible-HealthCheck]# vim hosts
# 内容如下
[k8s]
120.48.168.44
[k8s:vars]
ansible_ssh_user="root"
ansible_ssh_pass="你的主机密码"
```

## 报告及邮箱配置【可选】
```bash
vim /root/AnsibleHealthCheck/os-check/defaults/main.yaml，修改以下内容：
报告目录：check_report_path	设置报告生成目录（例如 /tmp）
SMTP相关变量（check_mail_host、check_mail_port、check_mail_username、check_mail_password、check_mail_to）根据实际情况填写
```

## 执行剧本，进行巡检
```bash
[root@instance-euwvmd1u AnsibleHealthCheck]# ansible-playbook -i hosts roles/os-check.yaml --limit k8s

PLAY [k8s] **********************************************************************************************************************************************************************************

TASK [os-check : Get system check data.] ****************************************************************************************************************************************************
changed: [120.48.168.44]

TASK [os-check : Generate report file.] *****************************************************************************************************************************************************
changed: [120.48.168.44]

TASK [os-check : Get report file content.] **************************************************************************************************************************************************
ok: [120.48.168.44]

TASK [os-check : Send a report by email.] ***************************************************************************************************************************************************
skipping: [120.48.168.44]

PLAY RECAP **********************************************************************************************************************************************************************************
120.48.168.44              : ok=3    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0 
```



## 配置hosts主机信息
```bash
# 前台执行
[root@instance-euwvmd1u AnsibleHealthCheck]# cd /tmp/ && python -m SimpleHTTPServer 30000
Serving HTTP on 0.0.0.0 port 30000 ...


# 后台运行
cd /tmp/ && nohup python -m SimpleHTTPServer 30000 >/dev/null2 >&1 &
```


## 配置hosts主机信息
```bash
http://120.48.168.44:30000/report-2025-12-26.html
```
