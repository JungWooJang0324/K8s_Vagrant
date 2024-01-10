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






  