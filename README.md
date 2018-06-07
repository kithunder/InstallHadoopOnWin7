# InstallHadoopOnWin7(windows7 安装 hadoop)
## 准备工作
1. Hadoop官方下载地址：http://hadoop.apache.org/releases.html 作者当时下载的是hadoop-3.0.2.zip
2. winutils下载：https://github.com/benwudashi/winutils 作者当时下载的其中3.0.0版本,据网友说2.9hadoop版本也可使用此版本的winutils。  
3. 将tar.gz包解压至不包含空格的路径：作者路劲为`D:/hadoop/hadoop-3.0.2`  
4. 配置环境变量：  
计算机（我的电脑）》属性》高级系统设置》环境变量》  
     新建变量 变量名：`HADOOP_HOME` 变量值：`D:/hadoop/hadoop-3.0.2`  
     Path环境变量末尾添加`;%HADOOP_HOME%\bin;%HADOOP_HOME%/sbin;`  
5. 安装Jdk到不包含空格的路径，作者路劲为：`C:/java/jdk1.8.0_171`  

## winutils 替换  
将`winutils/hadoop-3.0.0/bin/`下文件复制替换到`D:/hadoop/hadoop-3.0.2/bin` 即可

## Hadoop 配置
1. 修改D:/hadoop/hadoop-3.0.2/etc/hadoop/core-site.xml配置:
```
<configuration>  
    <property>  
       <name>fs.default.name</name>  
       <value>hdfs://localhost:9000</value>  
   </property>  
</configuration>
```   
2. 修改D:/hadoop/hadoop-3.0.2/etc/hadoop/mapred-site.xml配置:  
```
<configuration>
    <property>
       <name>mapreduce.framework.name</name>
       <value>yarn</value>
    </property>
</configuration>
```  
3. 在D:/hadoop/hadoop-3.0.2目录下创建data目录，作为数据存储路径：  
然后在data目录下分别创建 `namenode`,`datanode`目录  
4. 修改D:/hadoop/hadoop-3.0.2/etc/hadoop/hdfs-site.xml配置:  
```
<configuration>
 <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/D:/hadoop/hadoop-3.0.2/data/namenode</value>
    </property>
	<property>  
        <name>fs.checkpoint.dir</name>  
        <value>/D:/hadoop-3.0.2/data/snn</value>  
    </property>  
    <property>  
        <name>fs.checkpoint.edits.dir</name>  
        <value>/D:/hadoop-3.0.2/data/snn</value>  
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/D:/hadoop/hadoop-3.0.2/data/datanode</value>
    </property>
	<property>
     <name>dfs.permissions</name>
     <value>false</value>
  </property>
</configuration>
```  
5. 修改D:/hadoop/hadoop-3.0.2/etc/hadoop/yarn-site.xml配置:  
```
<configuration>
    <property>
       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
    </property>
    <property>
       <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
       <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```  
6. 修改D:/hadoop/hadoop-3.0.2/etc/hadoop/hadoop-env.cmd配置，找到`set JAVA_HOME=%JAVA_HOME%`替换为`set JAVA_HOME=C:/java/jdk1.8.0_171`  

## 启动服务  
1. D:/hadoop/hadoop-3.0.2/bin> `hdfs namenode -format`  
2. 通过start-all.cmd启动服务： D:/hadoop/hadoop-3.0.2/sbin> `start-all.cmd`  
3. 此时可以看到同时启动了如下4个服务：  
  -  Hadoop Namenode  
  -  Hadoop datanode  
  -  YARN Resourc Manager  
  -  YARN Node Manager

## HDFS应用  
1. 通过`http://127.0.0.1:8088/`即可查看集群所有节点状态：  
2. 访问`http://localhost:9870/`即可查看文件管理页面：  
   可以点击页面的Utilties>> borwse the file system 进行上传、下载、添加目录、删除目录等相关文件管理   
   **Note：在之前的版本中文件管理的端口是50070，在3.0.0中替换为了9870端口，具体变更信息来源如下官方说明
http://hadoop.apache.org/docs/r3.0.0/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html#Web_Interface**  
3. 通过hadoop命令行进行文件操作：  
  * mkdir命令创建目录：`hadoop fs -mkdir hdfs://localhost:9000/user`
  * put命令上传文件：`hadoop fs -put C:\Users\songhaifeng\Desktop\11.txt hdfs://localhost:9000/user/`
  * ls命令查看指定目录文件列表：`hadoop fs -ls hdfs://localhost:9000/user/`

  [替换后的handoop3.0.2](hadoop-3.0.2)
