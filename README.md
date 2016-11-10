Steps to set up Hortonworks HDP 2.5 in AWS (Centos 7 / RHEL 7.2)

Create AMI (Centos 7)

* 100 GB volume /dev/sdb: for hdfs
* 8 GB volume /dev/sdc: for ambari metrics

After EC2 Instance is up and running:

```
$ sudo mkfs -t ext4 /dev/xvdb
$ sudo mkfs -t ext4 /dev/xvdc
$ sudo mkdir /grid
$ sudo mkdir /metrics
$ sudo mount /dev/xvdb /grid
$ sudo mount /dev/xvdc /metrics
$ sudo echo "/dev/xvdb                                 /grid                   ext4    defaults,nofail 0 2" >> /etc/fstab
$ sudo echo "/dev/xvdc                                 /metrics                ext4    defaults,nofail 0 2" >> /etc/fstab
# Change umask to 022 vi /etc/profile
# Disable selinux /etc/sysconfig/selinux
$ sudo systemctl disable firewalld # Disable firewalld
$ sudo yum install ntp
$ sudo systemctl enable ntpd
$ sudo systemctl start ntpd
$ ssh-keygen -t rsa # Create password-less ssh keys
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ sudo yum erase snappy.x86_64 # HDP 2.5 depends on snappy-1.0.5. RHEL 7.2 comes with snappy-1.1.0 by default
$ sudo yum install wget
$ sudo wget -nv http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.21/repos/centos7/hdp-util.repo -O /etc/yum.repos.d/hdp-util.repo
$ sudo yum install snappy-devel-1.0.5-1.el6
```
Create AMI

