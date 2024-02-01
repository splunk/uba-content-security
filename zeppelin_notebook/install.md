# Zeppelin Installation on UBA 5.3.0+

The below steps were verified on:
* Oracle Linux Enterprise 8.8 - Single Node.
* Red Hat Enterprise Linux 8.6 - 7 Node Cluster.

### 1. Install Zeppelin
```
/opt/caspida/bin/zeppelin-notebook install 0.10.1
```

>[!NOTE]
>Or you may choose a different version if you'd like to explore that route.  See https://zeppelin.apache.org/download.html.


Example of expected output:
```
$ /opt/caspida/bin/zeppelin-notebook install 0.10.1
Installing Zeppelin notebook v0.10.1
Downloading from http://mirrors.ocf.berkeley.edu/apache/zeppelin/zeppelin-0.10.1/zeppelin-0.10.1-bin-all.tgz
zeppelin-0.10.1-bin 100%[===================>]   1.56G   431MB/s    in 5.5s

2024-01-22 13:33:13 (290 MB/s) - ‘/tmp/zeppelin/zeppelin-0.10.1-bin-all.tgz’ saved [1680577910/1680577910]

Downloading package                                        [  OK  ]
Installed at /var/vcap/packages/                           [  OK  ]
Zeppelin home is /var/vcap/packages/zeppelin-0.10.1-bin-all/
Setting port to 28080                                      [  OK  ]
Setting basic interpreter                                  [  OK  ]
Creating basic UBA notebook                                [  OK  ]
```

### 2. Modify the server address from file, /var/vcap/packages/zeppelin-0.10.1-bin-all/conf/zeppelin-site.xml

Update:
```
<property>
  <name>zeppelin.server.addr</name>
  <value>127.0.0.1</value>
  <description>Server binding address</description>
</property>

```

To:
```
<property>
  <name>zeppelin.server.addr</name>
  <value>enter_host_name OR ip_address</value>
  <description>Server binding address</description>
</property>
```

Example:
```
<property>
  <name>zeppelin.server.addr</name>
  <value>uba-host.com</value>
  <description>Server binding address</description>
</property>
```

> [!NOTE]
> You can also modify port value, search for property "zeppelin.server.port".
> Currently, this document only shows the installation method with HTTP communication, HTTPS communication is being tested.

### 3. Modify firewall to allow the default port (change port number if you’ve changed port number):

```
sudo firewall-cmd --zone=public --permanent --add-port 28080/tcp
sudo firewall-cmd --reload
```

### 4. Start Zeppelin
```
/opt/caspida/bin/zeppelin-notebook start
```

### 5. Navigate to your usual UBA host (non-ssl), http://<UBA_host>:<port number>/, example http://uba-host.com in the below example is what you will see:
![alt text](https://github.com/splunk/uba-content-security/blob/main/zeppelin_notebook/zeppelin-homepage.png)

### 6. Now, to add a note. Select "Import note" under "Notebook".  Select "JSON File/IPYNB" file then upload a .zpln format file.
![alt text](https://github.com/splunk/uba-content-security/blob/main/zeppelin_notebook/import-note.png)

![alt text](https://github.com/splunk/uba-content-security/blob/main/zeppelin_notebook/select-file.png)

### 7. Once imported all paragraphs will run for the first time.


### 8. You can create new notes that would use different interpreter, i.e. Spark, Python, Java. etc.
![alt text](https://github.com/splunk/uba-content-security/blob/main/zeppelin_notebook/new-note.png)