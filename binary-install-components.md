---
layout: default
title: 'HANA Express Components'
comments: true
# other options
---

## Install the Server and Application Package

### Install HANA Server and Database

Extract:
```
mkdir /opt/hxe/installer
tar -xvzf /opt/hxe/hxe.tgz -C /opt/hxe/installer
tar -xvzf /opt/hxe/hxexsa.tgz -C /opt/hxe/installer
```

Install:
```
cd /opt/hxe/installer
sudo ./setup_hxe.sh
```

Enter the values

Parameter          |values
-------------------|---------------------------------------
Root Directory     |/opt/hxe/installer/HANA_EXPRESS_20
System ID          |HXE
Instance Number    |90
Install Components |All
local host name    |<enter local host>
HDB master password|<enter in master password>

After install you should get back:

```
##################################
 # Summary after execution        #
 ##################################
 Server Installation...(OK)
 XSC Installation...(OK)
 XSA Installation...(OK)
 HXE Optimization...(OK)
```

Run the memory management script:

```
sudo su -l hxeadm

cd /usr/sap/HXE/home/bin
./hxe_gc.sh

exit
```

### Install SAP HANA HDB Client for Linux

Extract the contents of `clients_linux_x86_64.t` into /opt/hxe
```
tar -xvzf /opt/hxe/clients_linux_x86_64.tgz -C /opt/hxe
```

This extracts:
```
hdb_client_linux_x86_64.tgz : the SAP HANA HDB Client software package
xs.onpremise.runtime.client_linuxx86_64.zip : the SAP HANA XS CLI software package
```

Decompress the Client Package
```
tar -xvzf /opt/hxe/hdb_client_linux_x86_64.tgz -C /opt/hxe/installer
```

Run the Installer:
```
sudo su -l hxeadm

cd /opt/hxe/installer/HDB_CLIENT_LINUX_X86_64
./hdbinst

exit
```

Accept the defaults and enter installation path of:
Installation Path : /usr/sap/hdbclient

### Install SAP HANA External Machine Learning AFL

Extract:
```
tar -xvzf /opt/hxe/eml.tgz -C /opt/hxe/installer
```

Install:
```
sudo su -l hxeadm

cd /opt/hxe/installer/HANA_EXPRESS_20
./install_eml.sh

exit
```
After install you should receive message installation done.

Run the memory management script after this install
```
sudo su -l hxeadm

cd /usr/sap/HXE/home/bin
./hxe_gc.sh

exit
```

### SAP HANA Interactive Education (SHINE)

Extract:
```
tar -xvzf /opt/hxe/shine.tgz -C /opt/hxe/installer
```

Install:
```
sudo su -l hxeadm

cd /opt/hxe/installer/HANA_EXPRESS_20
./install_shine.sh

exit
```

Do not forget to record in password manager any passwords you create or use.

When install done you should get:

```
Software component XSAC_SHINE (sap.com) 'SAP Hana Demo Model for XS Advanced 1.0' successfully registered.
Process [install] finished successfully.
Performance /opt/hxe/installer/HANA_EXPRESS_20/DATA_UNITS/XSA_CONTENT_10/XSACSHINE04_2.ZIP: UploadFiles 9.99 s (9985 ms), Installation 9 m 49 s (589148 ms).
Installation of archive file '[/opt/hxe/installer/HANA_EXPRESS_20/DATA_UNITS/XSA_CONTENT_10/XSACSHINE04_2.ZIP]' finished successfully.
To see installation logs execute 'xs display-installation-logs 3001 -scv'.
````

Now run memory management:
```
sudo su -l hxeadm

cd /usr/sap/HXE/home/bin
./hxe_gc.sh

exit
```

### Install the additional Text Analysis files

```
sudo tar -xvzf /opt/hxe/additional_lang.tgz -C /hana/shared/HXE/global/hdb/custom/config/lexicon
```
