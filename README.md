# K8s_Vagrant
----------------------

## 1. Vagrant 설치 & Oracle VM Ware 설치 (이미 설치 되어있다면 최신 버전 확인)
```
[host@window10] vagrant --version  // Vagrant 2.4.0

[host@window10] mkdir [원하는 디렉토리] 
[host@window10] cd [지정한 디렉토리] 

// 지정한 디렉토리로 이동 후 초기화 vagrant init
[host@window10] vagrant init

[host@window10] cd [클론 받을 디렉토리로 이동]
[host@window10] git clone https://github.com/JungWooJang0324/K8s_Vagrant.git

```


클론된 디렉토리 -> vagrant/install로 이동

** 클론된 디렉토리의 Vagrantfile, ssh_config.sh, Vagrantfile_Ubuntu 복사 후 만든 디렉토리에 덮어쓰기 **


```
[host@window10] vagrant up
```

--------------------------

## 2. 서버 사전준비 - Master, Node 모두

* root계정 password 지정 (vagrant로 통일)
```
[host@window10] vagrant ssh [원하는 서버 이름]

[host@window10] sudo passwd root
[host@window10] su - root
[host@window10] getenforce 
Enforcing //기본값. 보안 정책에 맞지 않는 요청은 거부된다.: enforcing
[host@window10] setenforce 0
[host@window10] sestatus
[host@window10] sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
[host@window10] getenforce
Permissive //경고만한다

```


* 방화벽 해제
```
[host@window10] systemctl status firewalld
[host@window10] systemctl stop firewalld && systemctl disable firewalld
[host@window10] systemctl stop NetworkManager && systemctl disable NetworkManager
```

* swap 비활성화 & Iptables 커널 옵션 활성화
```
[host@window10] swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab

[host@window10] cat <<EOF>>  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF


[host@window10] sysctl --system
```


* 쿠버네티스를 위한 yum레파지토리 설정

```
[host@window10] cat <<EOF>> /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=0
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```

* Centos UPdate
```
[host@window10] yum update
```

* hosts파일 변경 
```
[host@window10] vi /etc/hosts

// 뒤에 hostsname ip 수정
...

192.168.32.10 k8s-master
192.168.32.11 k8s-node01
192.168.32.12 k8s-node02

```
* 서버 재실행


--------------------------
