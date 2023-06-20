
# ansible

## ansible特点

* 安装部署简单：ansible只需在主控端部署环境，被控主机无需任何配置
* 基于SSH进行配置管理，
* 不需要守护进程
* 日志集中存储
* Ansible使用YAML语法管理配置
* 通过模块来实现各种功能
* ansible具有冥等性，如果执行结果与当前状态相同将不做操作。且返回值为绿色

### ansible使用软件

* 使用阿里云yum源
* yum -y install epel-release （安装扩展仓库）
* yum -y install ansible-core  （安装ansible核心模块）
* yum -y install ansible （安装ansible）

### ansible文件介绍

#### /etc/ansible/hosts

```shell

#定义主机群的名字
[主机分类]
#主机群里的主机，可以是ip或域名，设置登陆用户和密码。默认不写是当前用户和默认ssh端口，主配置文件可以更改默认配置
# 主机后面的参数
ansible_ssh_pass
ansible_become
ansible_become_method = su/sudo
ansible_become_user
ansible_become_pass

192.168.88.20  ansible_user=root  ansible_port=22
www.web.com 

[webs]
192.168.88.21
192.168.88.32
```

ansible默认被控主机清单

#### /etc/ansible/ansible.cfs

ansible配置文件

```s

#默认配置
[defaults]
#设置链接主机默认配置端口和用户
remote_port = 123
remote_user = adc

#使用当前工作目录的主机清单
inventory = ./inventory

# 不启用ssh的known_hosts检查
host_key_checking = False 

#遇到错误继续执行
force_handlers = True


[privilege_escalation]
#表示提权
become = true
#提示输入提权密码（有争议，不懂诶）
#但是可以在执行时使用 --ask-become-pass 来提示输入密码
become-ask-pass = true
#修改提权用户
become_user = named
#存放提权用户的密码
become_password_file = ./become_passwd
```

#### playbook剧本文件

yml语法在文本的后面不能有空格
\-后面有一个空格，如果冒号后面没字符串不能跟空格
\-的下面一行如果是下一级数据必须和name齐平
\-必须和上面一行的文本开头齐平
模板

```yaml
- name: 描述信息
  hosts: webs
  vars:
    var_name: var_value
  tasks:
  - name: 1
    shell: id
  - name: 2
    yum:
      name: tree
      state: installed
  - name:
    shell: tree -gpu /usr/bin

```

```yaml
#遇到错误继续执行
ignore-errors=true
- name: 脚本描述信息
  hosts: 执行主机
  #所有主机启用sudo
  become: true 
  vars: 
  #定义变量
    var_name: var_value
  tasks: 
  - name: 查看当前用户
    shell: id
  - name: 安装tree
    yum: 
      name: tree
      state: installed
    notify: 
    - 查看是否正常
  handlers: 
  - name: 查看是否正常
    shell : ls -alh
```

### ansible命令

* ansible 被控主机  -m  使用模块  -a 参数
  * --list-hosts  （列出服务器列表）
  * -i  列出指定服务器列表
    * ansible all -i /etc/hosts --list-hosts （查看这个文件的所有主机，可以指定一个规定的主机群）

基础参数

* -k，--ask-pass 登录密码，提示输入SSH密码而不是假设基于密钥的验证
* --ask-su-pass su切换密码
* -K，--ask-sudo-pass 提示密码使用sudo，sudo表示提权操作
* --ask-vault-pass 假设我们设定了加密的密码，则用该选项进行访问
* -B SECONDS #后台运行超时时间
* -C #模拟运行环境并进行预运行，可以进行查错测试
* -c CONNECTION #连接类型使用
* -f FORKS #并行任务数，默认为5
* --list-hosts #查看有哪些主机组
* -o #压缩输出，尝试将所有结果在一行输出，一般针对收集工具使用
* -S #用 su 命令
* -s #用 sudo 命令
* -R SU_USER #指定 su 的用户，默认为 root 用户
* -U SUDO_USER #指定 sudo 到哪个用户，默认为 root 用户
* -T TIMEOUT #指定 ssh 默认超时时间，默认为10s，也可在配置文件中修改
* -u REMOTE_USER #远程用户，默认为 root 用户
* -v #查看详细信息，同时支持-vvv，-vvvv可查看更详细信息
