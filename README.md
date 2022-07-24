# Ansible Guide

## _About Ansible_

> Ansible is an IT automation tool. It can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.

## Benefits

* Automation
* Agentless
* Infrastructure as code (IaC)
* Playbooks are written in YAML

## Mechanism

![ansible-pic-1](https://miro.medium.com/max/700/1*rhVOlEgI8a_XQrw-Ick0ig.png)
![ansible-pic-2](https://www.netadmin.com.tw//netadmin/img/cms/0/0/1/2020-07/202007311547137185188258.jpg)

## _Ansilbe Playbooks_

> Playbooks are one of the core features of Ansible and tell Ansible what to execute. They are like a to-do list for Ansible that contains a list of tasks.

Playbooks are written in YAML format and run sequentially.

## _Ansilbe Role_

> Roles provide a framework for fully independent, or interdependent collections of variables, tasks, files, templates, and modules.

In Ansible, the role is the primary mechanism for `breaking a playbook into multiple files`. This simplifies writing complex playbooks, and it makes them easier to reuse. The breaking of playbook allows you to logically break the playbook into `reusable components`.

## _ansible-galaxy_

> command to manage Ansible roles in shared repositories, the default of which is Ansible Galaxy <https://galaxy.ansible.com>.

## Installation

配置PPA及安裝 Ansible

```sh
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

## Pre-Configuration

將想要託管的節點 host-name 加入控制節點的 /etc/hosts中

## Generate Key

使用 ssh-keygen 產生金鑰並存放於 /home/osadmin/.ssh/ansible

```sh
ssh-keygen
```


把 id_rsa 改成 ansible



## Pass public key to managed hosts

為了要讓控制節點(Control node)能透過 ssh 連線上託管節點, 需要將產生的 ssh-key 放置到託管節點(Managed nodes)

* 方法一 : 直接將 public key 的內容貼到託管節點的 ~/.ssh/authorized_keys 中
* 方法二 : 使用 ssh-copy-id 傳送到託管節點

```sh
ssh-copy-id -i ansible {託管節點-host-name}
```

# sudo usercat simplifi

由於在執行 Ansible 模組時幾乎都會使用 sudo 權限所以需要在託管節點開放當前使用者sudo 時不用輸入密碼, 以 osadmin 為例

```sh
sudo visudo
```

> 修改 osadmin 的內容為 `osadmin ALL=(ALL:ALL) NOPASSWD:ALL`

## 常用指令

#### 確認 hosts 中的 managed hosts 通訊是正常的

```sh
ansible [hostname/groupname] -m ping -i [hosts-file-path]
```

#### 確認 managed hosts 是否都可以不用輸入 sudo 密碼

```sh
ansible [hostname/groupname] -a "grep ^root: /etc/shadow" -i [hosts-file-path]
```

#### 執行 ansible-playbook

```sh
ansible-playbook [site-file-path]
```

#### 下載並解壓縮至 /home/osadmin/.ansible/roles 並安裝 ansible roles

```sh
ansible-galaxy install [local / remote role file]
```

#### 顯示安裝的 ansible roles 清單以及存放的位置

```sh
ansible-galaxy list
```

## Reference Link

* [《現代 IT 人一定要知道的 Ansible 自動化組態技巧》](https://chusiang.gitbooks.io/automate-with-ansible/content/) by 凍仁翔

**Enjoy!**
