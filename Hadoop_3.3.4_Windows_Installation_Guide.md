
# 🐘 Hadoop 3.3.4 Installation Guide on Windows (No WSL)

This guide walks you through installing Hadoop 3.3.4 on Windows with UI access and proper winutils setup.

---

## ⚠️ Pre-requisites

- **Uninstall any older Hadoop installations.**
- Make sure you have `Admin` access on your system.

## 📥 Downloads

| Tool                     | Description                          | Link                                      |
|--------------------------|--------------------------------------|-------------------------------------------|
| JDK 1.8 (Update 361)     | Required by Hadoop                   | [Download JDK 1.8_361](https://drive.google.com/file/d/1MG3shs65Zpb-ZR_11GUM3WD7VSoGENfQ/view) |
| WinRAR                   | To extract `.tar.gz` files           | [Download WinRAR](https://www.win-rar.com/fileadmin/winrar-versions/winrar/winrar-x64-620.exe) |
| Hadoop 3.3.4             | Core Hadoop binaries                 | [Download Hadoop](https://hadoop.apache.org/release/3.3.4.html) |
| bin.zip (winutils)       | Windows-compatible Hadoop binaries   | [Download bin.zip](https://github.com/Suriya2210/Installing_Hadoop_3.3.4_Win10-11NoWSL/raw/main/Resources/bin.zip) |

---

## 🔧 Installation Steps

### 1. 🛠 Install JDK
- Run `jdk-8u361-windows-x64.exe`
- Use default path or `C:\Program Files\Java\jdk1.8.0_361`

### 2. 📦 Extract Hadoop
- Use WinRAR to extract `hadoop-3.3.4.tar.gz` to `C:\` directly.

### 3. ⚙️ Set Environment Variables
- Open **System Properties > Environment Variables**
- Add:
  - `JAVA_HOME` = `C:\Program Files\Java\jdk1.8.0_361`
  - `HADOOP_HOME` = `C:\hadoop-3.3.4`
- Edit `Path` and append:
  - `%JAVA_HOME%\bin`
  - `%HADOOP_HOME%\bin`
  - `%HADOOP_HOME%\sbin`

### 4. 🧩 Add winutils.exe
- Extract `bin.zip` and paste `bin` into `C:\hadoop-3.3.4`.

### 5. 📁 Create Hadoop Directories
```bash
C:\hadoop-3.3.4\data\namenode
C:\hadoop-3.3.4\data\datanode
```

### 6. ⚙️ Configure `core-site.xml`
Paste inside:
```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

### 7. ⚙️ Configure `mapred-site.xml`
```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

### 8. ⚙️ Configure `hdfs-site.xml`
```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/hadoop-3.3.4/data/namenode</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/hadoop-3.3.4/data/datanode</value>
  </property>
</configuration>
```

### 9. ⚙️ Configure `yarn-site.xml`
```xml
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
</configuration>
```

### 10. 🔁 Update `hadoop-env.cmd`
Edit file: `C:\hadoop-3.3.4\etc\hadoop\hadoop-env.cmd`
- Replace:
```bat
set JAVA_HOME=%JAVA_HOME%
```
- With:
```bat
set JAVA_HOME=C:\Progra~1\Java\jdk1.8.0_361
```
- If username has space, set:
```bat
set HADOOP_IDENT_STRING=<ModifiedUserNameLikeHar~1>
```

### 11. ✅ Verify Installation
```cmd
# Run as Administrator
hdfs version
hdfs namenode -format
start-all.cmd
```

Visit:
- NameNode UI: [http://localhost:9870](http://localhost:9870)
- ResourceManager UI: [http://localhost:8088](http://localhost:8088)

---

## 🛑 Stop Hadoop
```cmd
stop-all.cmd
```

---

## 🧪 Troubleshooting Tips
- `JAVA_HOME` errors → Check env vars + `hadoop-env.cmd`
- Ports not opening → Disable firewall or allow Java access
