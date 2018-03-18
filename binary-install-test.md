## Testing The installation

### Running Processes

```
sudo su -l hxeadm
HDB info | grep -e hdbnameserver -e hdbcompileserver -e hdbindexserver -e hdbdiserver -e hdbwebdispatcher
```

You should see:
```
hxeadm    71252  71235 55.2 7982280 5377200      \_ hdbnameserver
hxeadm    71347  71235  2.1 1580412 257312      \_ hdbcompileserver
hxeadm    71374  71235 12.8 5321396 2845732      \_ hdbindexserver -port 39003
hxeadm    71516  71235  1.6 1588012 260768      \_ hdbdiserver
hxeadm    71518  71235  1.9 1880592 279788      \_ hdbwebdispatcher
```

Important note - the instructions have come out earlier than other Express editions.  Since the source of these instructions, there should be more components activated.  AT the time of this document you should see the following:


The following services must be running:

- hdbnameserver
- hdbcompileserver
- hdbindexserver
- hdbdiserver
- hdbwebdispatcher

Execute:

```
HDB start
```

SAP HANA CLIENT

- JDBC
- Python
- ODBC
- SQLDBC
- Node.js
- Ruby (content coming soon!)

### SAP HANA External Machine Learning Library (EML)

```
sudo su -l hxeadm

cd /usr/sap/HXE/HDB90/exe

./hdbsql \
	-i 90 \
	-d SystemDB \
	-u SYSTEM \
	-p "<master-password>" \
	"SELECT * FROM SYS.AFL_AREAS WHERE AREA_NAME = 'EML';"
```

You should get back:
```
AREA_OID,SCHEMA_NAME,AREA_NAME,CREATE_TIMESTAMP
159136,"_SYS_AFL","EML","<installation date>"
```

### XS Engine

Verify that the XS Engine is up and running graphic is returned:

http://<hostname>:8090

or

https://<hostname>:4390

If you can't connect:

```
netstat -ano | grep -e 8090 -e 4390 | grep LISTEN
```

You should get:
```
tcp   0   0 0.0.0.0:8090    0.0.0.0:*     LISTEN   off (0.00/0/0)
tcp   0   0 0.0.0.0:4390    0.0.0.0:*     LISTEN   off (0.00/0/0)
```

That means a service is off or firewall is causing problem during install.


### XSA Engine

Verify https://<hostname>:39030 is up and Running

You should get a main page graphic with XSA is up and running.

If it cannot connect:

```
netstat -ano | grep -e 39030 | grep LISTEN
```

Expected results:
```
tcp   0   0 0.0.0.0:39090    0.0.0.0:*     LISTEN   off (0.00/0/0)
```

If the result is empty, then one of the required services is not up and running else your firewall is blocking the communication

### XSA Admin Cockpit

```
sudo su -l hxeadm
xs login -u xsa_admin -p "<master-password>"
```

Then run
```
xs apps
```


This command may fail due to a missing SSL certificate.

To fix this problem, you will need to set up the certificate for your client.
The above example show the local path, therefore if you want to run the following from a different machine you will have to transfer the available on the SAP HANA, express edition server machine to the client machine first and adjust the command path.

```
xs api https://<hostname>:39030 -cacert /hana/shared/HXE/xs/controller_data/controller/ssl-pub/router/default.root.crt.pem
```

```
xs apps | grep cockpit-admin-web-app
```

Should get message started.

logon with  XSA_ADMIN and password

When you connect for the first time, you will receive a message regarding missing COCKPIT_ADMIN role collection. Accept the COCKPIT_ADMIN role collection to be added to the XSA_ADMIN user.

Logout and relogin:
Login again using XSA_ADMIN as the username and the master password.


### SAP Web IDE for SAP HANA

```
sudo su -l hxeadm

xs login -u xsa_admin -p "<master-password>"
```

```
xs apps | grep webide
```

Get the URL:

```
xs app webide -urls
```

This should give you access to the SAP Web IDE for SAP HANA login screen.

Use the XSA_DEV user and credentials that was created during the installation process to login.



### SAP HANA Interactive Education (SHINE)

```
sudo su -l hxeadm

xs login -u xsa_admin -p "<master-password>"
xs app shine-web -urls
```

Where <master-password> is the XSA_ADMIN password which should be the master password:

You will get the SHINE application URL. Open the returned URL in your browser.

This should give you access to the SAP HANA Interactive Education login screen.
