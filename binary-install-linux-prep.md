
LINUX system preparation

Run this command:
```
sudo yum repolist
```
You should see output of `RHEL EUS Server SAP HANA`

Next clean the cache:
```
sudo yum clean all
```

Environment Packages - Base
Get available packages:
```
sudo yum grouplist
```

Mark base package:
```
sudo yum group mark convert
```

If not installed, install it:
```
sudo yum -y groupinstall base
```

Verify the following packages are installed by issuing:
```
rpm -qa --queryformat '(%{installtime:date}) %{name} %{version}\n' kernel
rpm -qa --queryformat '(%{installtime:date}) %{name} %{version}\n' kexec-tools
rpm -qa --queryformat '(%{installtime:date}) %{name} %{version}\n' glibc
```

Next install the following packages:
```
sudo yum -y install xulrunner \
	sudo \
	libssh2 \
	expect \
	graphviz \
	iptraf-ng \
	krb5-workstation \
	libpng12 \
	nfs-utils \
	lm_sensors \
	openssl \
	PackageKit-gtk3-module \
	libcanberra-gtk2 \
	xorg-x11-xauth \
	numactl \
	bind-utils
```

Verify the RHEL SAP HANA Compatibility Library
```
rpm -qa --queryformat '(%{installtime:date}) %{name}\n' compat-sap-c++-6
```
Output should be 6.3.1 or higher

If it is missing or less than version 6.3.1 then:
```
sudo yum -y install compat-sap-c++-6
```

Activate the RHEL SAP HANA Specific Tuned Profiles
```
sudo yum -y install tuned-profiles-sap-hana
```

Now restart:
```
sudo systemctl start tuned
sudo systemctl enable tuned
```

Check the service status:
```
sudo systemctl status tuned
```

Verify you can execute the profile:
```
tuned-adm profile sap-hana
```

or if on VMware:
```
tuned-adm profile sap-hana-vmware
```

Disable SELinux:
```
sudo setenforce 0
sudo sed -i 's/\(SELINUX=enforcing\|SELINUX=permissive\)/SELINUX=disabled/g' /etc/selinux/config
```

Check the service status:
```
sestatus
```

It should return:
```
SELinux status:                 disabled
```

Check status:
```
getenforce
```

It should return:
```
Disabled
```

Disable Disable Automatic NUMA Balancing
```
sudo cp /etc/sysctl.d/sap_hana.conf /etc/sysctl.d/sap_hana.conf.bkp
sudo echo "kernel.numa_balancing = 0" > /etc/sysctl.d/sap_hana.conf
sudo sysctl -p /etc/sysctl.d/sap_hana.conf
```

If RHEL7 then also:
```
sudo systemctl stop numad
sudo systemctl disable numad
```

Check if symbolic links exist:
```
ls -la /usr/lib64/libssl.so.1.0.1 /usr/lib64/libcrypto.so.1.0.1
```

Make sure they show:
```
lrwxrwxrwx. 1 root root XX yyy zz aa:bb /usr/lib64/libcrypto.so.1.0.1 -> /usr/lib64/libcrypto.so.1.0.1e
lrwxrwxrwx. 1 root root XX yyy zz aa:bb /usr/lib64/libssl.so.1.0.1 -> /usr/lib64/libssl.so.1.0.1e
```

If not present then make them:
```
sudo ln -s /usr/lib64/libssl.so.1.0.1e /usr/lib64/libssl.so.1.0.1
sudo ln -s /usr/lib64/libcrypto.so.1.0.1e /usr/lib64/libcrypto.so.1.0.1
```

Disable firewall to avoid issues:
```
sudo systemctl stop firewalld.service
sudo systemctl disable firewalld.service
```

Check the Java Version:
```
java -version
```

Which should return:
```
java version "1.8.0_xx"
Java(TM) SE Runtime Environment (build 1.8.0_xx-yyy)
```

If not installed [see](https://tools.hana.ondemand.com/#cloud)

RPM method preferred run:
```
sudo rpm -ivh <rpm directory>/sapjvm-<version>-linux-x64.rpm
```

Update the alternatives Java components:
```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/java/sapjvm_8_latest/bin/java" 1
sudo update-alternatives --set java /usr/java/sapjvm_8_latest/bin/java
```

Reboot the machine.
