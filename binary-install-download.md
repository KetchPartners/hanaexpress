

Register the SAP HANA Express Install:
[Register Here](https://www.sap.com/developer/topics/sap-hana-express.html)

Make sure to select:
HANA 2.0 Express Server and Applications for On Premise LINUX

Download directly to the RHEL Linux server if not connected to internet than you can do locally than file transfer it to linux server.

Use the platform independent:
```
java -jar <download manager path>/HXEDownloadManager.jar -h
```

How to download:
```
HXEDownloadManager [( [-h|-X] | [-d <save_directory>] [--ph <proxy_host>] [--pp <proxy_port>] <platform> <image> <file>... )]
```

You will need to download the following packages:

- Server Only Installer
- Applications
- Client for Linux X86/64
- SAP HANA Interactive Education (SHINE)
- SAP HANA External Machine Learning AFL
- Text analysis files for additional languages


Issue the command to download:
```
sudo mkdir /opt/hxe
sudo chmod a+rwx /opt/hxe

cd <download manager path>

java -jar HXEDownloadManager.jar linuxx86_64 installer \
  -d /opt/hxe \
  --ph proxy \
  --pp 8080 \
  hxe.tgz \
  hxexsa.tgz \
  clients_linux_x86_64.tgz \
  shine.tgz \
  eml.tgz \
  additional_lang.tgz
```
