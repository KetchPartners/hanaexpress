## Deactivate the SYSTEM user:

The SYSTEM user is the database “super-user” and is not intended for day-to-day activities in production systems.

For better security, you can create other database users with only the privileges that they require for their tasks (for example, user administration), then deactivate the SYSTEM user.

You need now to execute the following commands to create a new admin user with the USER ADMIN system privilege

```
sudo su -l hxeadm

cd /usr/sap/HXE/HDB90/exe

./hdbsql \
	-i 90 \
	-d SystemDB \
	-u SYSTEM \
	-p "<master-password>" \
	"CREATE USER <admin-username> PASSWORD <admin-password> NO FORCE_FIRST_PASSWORD_CHANGE;"

./hdbsql \
	-i <instance-number> \
	-d SystemDB \
	-u SYSTEM \
	-p "<master-password>" \
	"GRANT USER ADMIN TO <admin-username> WITH ADMIN OPTION;"
```

Create at least 5 admin users.


Deactivate SYSTEM user:

```
./hdbsql \
  -i <instance-number> \
  -d SystemDB \
  -u SYSTEM \
  -p "<master-password>" \
	"ALTER USER SYSTEM DEACTIVATE USER NOW;"
```
