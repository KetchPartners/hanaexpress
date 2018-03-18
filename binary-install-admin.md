---
layout: default
title: 'HANA Express Administration'
comments: true
# other options
---


## XSA Administration

### Backup

Now take a backup according to the admin guide:

[HANA Express Admin Guide](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)


### Memory

You can set how much memory SAP HANA, express edition utilizes by modifying the global_allocation_limit parameter in the global.ini file.

The measure unit for the global_allocation_limit parameter is MB.

The default value is 0, which sets the maximum memory to the minimum of your machine limit and license limit.

If the machine size is less than 16 GB, the maximum memory is set to 16 GB.

If you set global_allocation_limit to a non-zero value, the server instance will use that value as maximum memory.

Here an example command to alter the global_allocation_limit value to 16 GB (to be executed as hxeadm user):

```
cd /usr/sap/HXE/HDB90/exe
./hdbsql \
	-i 90 \
	-d SystemDB \
	-u SYSTEM \
	-p "<master_password>" \
  "alter system alter configuration ('global.ini','SYSTEM') set ('memorymanager', 'global_allocation_limit') = '16384' with reconfigure;"
```


### Memory Management

Run the Memory Management script:

```
sudo su -l hxeadm

cd /usr/sap/HXE/home/bin
./hxe_gc.sh

```


### other

Review the guide in detail and incorporate into operations plan:

[HANA Express Admin Guide](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)


### firewall

This is the recommended firewall config:

```
sudo echo "<?xml version=\"1.0\" encoding=\"utf-8\"?>" > /etc/firewalld/services/hana.xml
sudo echo "<service>" >> /etc/firewalld/services/hana.xml
sudo echo "	<short>SAP HANA</short>" >> /etc/firewalld/services/hana.xml
sudo echo "	<description>Firewall rules for SAP HANA</description>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"1128\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"1129\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"4390\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"8090\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"30000-39999\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "	<port port=\"50000-59999\" protocol=\"tcp\"/>" >> /etc/firewalld/services/hana.xml
sudo echo "</service>" >> /etc/firewalld/services/hana.xml

sudo chmod 0640 /etc/firewalld/services/hana.xml

sudo systemctl start firewalld
sudo systemctl enable firewalld

sudo firewall-cmd --zone=public --add-service=hana --permanent
sudo firewall-cmd --reload
```


Check the status
```
sudo firewall-cmd --list-services
```
