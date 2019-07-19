# Hadoop-quickstart



## 1. 环境搭建



### 1.1 虚拟机环境准备

1. 克隆虚拟机 

2. 修改克隆虚拟机的静态IP

3. 修改主机名

4. 关闭防火墙

5. 创建csy用户

6. 配置csy用户具有root权限

7. 在/opt目录下创建文件夹

   - 在/opt目录下创建module、software文件夹

     `sudo mkdir module`
     `sudo mkdir software`

   - 修改module、software文件夹的所有者cd

     `[csy@hadoop101 opt]$ sudo chown csy:csy module/ software/`



### 1.2 安装JDK

1. 卸载现有的JDK

   - 查询是否安装Java软件：

     `rpm -qa | grep java`

   - 如果安装的版本低于1.7，卸载该JDK：

     `sudo rpm -e 软件包`

   - 查看JDK安装路径

     `which java`

2. 将JDK导入到opt目录下的softaware文件夹下面

3. 解压JDK到/opt/module目录下

4. 配置JDK环境变量

   - 打开*/etc/profile*文件，在profile文件末尾添加JDK路径

     ```properties
     #JAVA_HOME
     export JAVA_HOME=/opt/module/jdk1.8.0_144
     export PATH=$PATH:$JAVA_HOME/bin
     ```

   - 保存退出，然后使其生效 `source /etc/profile`

   - 检测JDK是否安装成功 `java -version`

   > 如果不知道jdk 路径可以进入到jdk解压目录 然后使用 `pwd`命令



### 1.3 安装Hadoop

1. 可以在[Hadoop官网](https://archive.apache.org/dist/hadoop/common/hadoop-2.7.2/)去下载编译好的包

2. 进入到Hadoop安装包路径下（这里所有软件放在*/opt/software*文件夹下）

3. 解压安装文件到*/opt/module*下面

4. 查看是否解压成功，并将Hadoop添加到环境变量

   - 获取Hadoop的安装路径（/opt/module/hadoop-2.7.2）

   - 打开/etc/profile文件，在文件末尾添加Hadoop路径

     ```properties
     #HADOOP_HOME
     export HADOOP_HOME=/opt/module/hadoop-2.7.2
     export PATH=$PATH:$HADOOP_HOME/bin
     export PATH=$PATH:$HADOOP_HOME/sbin
     ```

   - 保存退出，并使其生效

   - 测试是否安装成功 `hadoop version`



### 1.4 Hadoop目录结构

1. 查看Hadoop目录结构

   ![1563354324895](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563354324895.png)

2. 重要目录

   | bin目录   | 存放对Hadoop相关服务（HDFS,YARN）进行操作的脚本 |
   | --------- | ----------------------------------------------- |
   | etc目录   | Hadoop的配置文件目录，存放Hadoop的配置文件      |
   | lib目录   | 存放Hadoop的本地库（对数据进行压缩解压缩功能）  |
   | sbin目录  | 存放启动或停止Hadoop相关服务的脚本              |
   | share目录 | 存放Hadoop的依赖jar包、文档、和官方案例         |



## 2. Hadoop 运行模式

Hadoop运行模式包括：本地模式、伪分布式模式以及完全分布式模式。

Hadoop官方网站：http://hadoop.apache.org/



### 2.1 本地运行模式

#### 2.1.1 官方Grep案例

1. 创建在hadoop-2.7.2文件下面创建一个input文件夹

2. 将Hadoop的xml配置文件复制到input

   `cp etc/hadoop/*.xml input`  

3. 执行share目录下的MapReduce程序

   `hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar gerp input output 'dfs[a-z.]+'`

4. 查看输出结果

   `cat output/*`

#### 2.1.2 官方WordCount案例

1. 创建在hadoop-2.7.2文件下面创建一个wcinput文件夹
2. 在wcinput文件下创建一个wc.input文件
3. 编辑wc.input文件（输入任意内容，以便后面进行单词数量计算）
4. 回到Hadoop目录/opt/module/hadoop-2.7.2
5. 执行 `hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount wcinput wcoutput`
6. 查看结果
   `cat wcoutput/part-r-00000`



### 2.2 伪分布式运行模式

#### 2.2.1 启动HDFS并运行MapReduce程序

1. 分析

   - 配置集群
   - 启动、测试集群增、删、改、
   - 执行WordCount案例

2. 执行步骤

   - 配置集群

     - 配置：hadoop-env.sh

       Linux系统中获取JDK的安装路径：`echo $JAVA_HOME`

       修改JAVA_HOME路径：
       `exprot JAVA_HOME=/opt/module/jdk1.8.0_144`

     - 配置：core-site.xml

       ```xml
       <!-- 指定HDFS中NameNode的地址 -->
       <property>
       <name>fs.defaultFS</name>
           <value>hdfs://hadoop101:9000</value>
       </property>
       
       <!-- 指定Hadoop运行时产生文件的存储目录 -->
       <property>
       	<name>hadoop.tmp.dir</name>
       	<value>/opt/module/hadoop-2.7.2/data/tmp</value>
       </property>
       ```

     - 配置：hdfs-site.xml

       ```xml
       <!-- 指定HDFS副本的数量 -->
       <property>
       	<name>dfs.replication</name>
       	<value>1</value>
       </property>
       ```

   - 启动集群

     - **格式化NameNode**（第一次启动时格式化，格式化之会初始化一个*data\*目录）
       `hdfs namenode -format`
     - 启动NameNode
       `hadoop-daemon.sh start namenode`
     - 启动DataNode
       `hadoop-daemon.sh start datanode`

   - 查看集群

     - 查看是否启动成功 （使用`jps`命令）
     - 使用web端查看HDFS文件系统（http://hostname:50070）
     - 查看产生的Log日志 `/opt/module/hadoop-2.7.2/logs`

   - 操作集群

     - 在HDFS文件系统上创建一个input文件夹
       `hdfs dfs -mkdir -p /user/csy/input`

     - 将测试文件内容上传到文件系统上
       `hdfs dfs -put wcinput/wc.input /user/csy/input/`

     - 查看上传的文件是否正确
       `hdfs dfs -ls /user/csy/input/`
       `hdfs dfs -cat /user/csy/input/wc.input`

     - 运行MapReduce程序
       `hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.7.jar wordcount /user/csy/input/ user/csy/output`

     - 查看输出结果
       `hdfs dfs -cat /user/atguigu/output/*`

       浏览器查看（http://hostname:50070）

       ![1563359917192](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563359917192.png)

     - 将测试文件内容下载到本地
       `[csy@hadoop101 hadoop-2.7.2]$ hdfs dfs -get /user/csy/output/part-r-0000 ./wcoutput/`

     - 删除输出结果
       `hdfs dfs -rm -r /user/csy/output`

   

#### 2.2.2 启动YARN并运行MapReduce程序

1. 分析

   （1）配置集群在YARN上运行MR

   （2）启动、测试集群增、删、查

   （3）在YARN上执行WordCount案例

2. 执行步骤

   （1）配置集群

   ​	（a）配置*yarn-env.sh*

   ​		配置*JAVA_HOME*
   ​		`exprot JAVA_HOME=/opt/module/jdk1.8.0_144`

   ​	（b）配置*yarn-site.xml*

   ```xml
   <!-- Reducer获取数据的方式 -->
   <property>
    		<name>yarn.nodemanager.aux-services</name>
    		<value>mapreduce_shuffle</value>
   </property>
   
   <!-- 指定YARN的ResourceManager的地址 -->
   <property>
   	<name>yarn.resourcemanager.hostname</name>
   	<value>hadoop101</value>
   </property>
   ```

   ​	（c）配置：*mapred-env.sh*

   ​		配置一下JAVA_HOME

   ​		`export JAVA_HOME=/opt/module/jdk1.8.0_144`

   ​	（d）配置：*mapred-site.xml*（对*mapred-site.xml.template*重命名为*mapred-site.xml*）
   ​		`mv mapred-site.xml.template mapred-site.xml`
   ​		`vim mapred-site.xml`

   ```xml
   <!-- 指定MR运行在YARN上 -->
   <property>
   		<name>mapreduce.framework.name</name>
   		<value>yarn</value>
   </property>
   ```

   （2）启动集群

   ​	（a）启动前必须保证**NameNode**和**DataNode**已经启动

   ​	（b）启动ResourceManager
   ​		`yarn-daemon.sh start resroucemanager`

   ​	（c）启动NodeManager
   ​		`yarn-daemon.sh start nodemanager`

   （3）集群操作

   ​	（a）YARN的浏览器页面查看，如图（http://hostname:8088）
   ![1563364124543](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563364124543.png)

   ​	（b）删除文件系统上的output文件
   ​		`hdfs dfs -rm -R /user/csy/output`

   ​	（c）执行MapReduce程序
   ​		`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/csy/input /user/csy/output`

   ​	（d）查看运行结果，如下图
   ​		`hdfs dfs -cat /user/csy/output/*`

#### 问题

**hadoop 50070无法访问的问题的解决方法**

1. ```shell
   [csy@hadoop101 hadoop]# vim /etc/selinux/config
   #将SELINUX改为disabled
   SELINUX=disabled
   ```

2. 检查*$HADOOP_HOME/etc/hadoop*下的***core-site.xml***和***hdfs-site.xml***是否配置好

3. 必须在hadoop-env.sh文件中设置Java的绝对路径

4. 是否关闭linux系统的防火墙（service iptables status）

5. 查看你windows里本地的配置文件的IP和主机名映射关系



#### 2.2.3 配置历史服务器

为了查看程序的历史运行情况，需要配置一下历史服务器。具体配置步骤如下：

1. 配置*mapred-site.xml*

   ```xml
   <!-- 历史服务器端地址 -->
   <property>
   <name>mapreduce.jobhistory.address</name>
   <value>hadoop101:10020</value>
   </property>
   <!-- 历史服务器web端地址 -->
   <property>
       <name>mapreduce.jobhistory.webapp.address</name>
       <value>hadoop101:19888</value>
   </property>
   ```

2. 启动历史服务器
   `mr-jobhistory-daemon.sh start historyserver`

3. 查看历史服务器是否启动 `jps`

4. 查看JobHistory http://hadoop101:19888/jobhistory



#### 2.2.4 配置日志聚集

日志聚集概念：应用运行完成以后，将程序运行日志信息上传到HDFS系统上。

日志聚集功能好处：可以方便的查看到程序运行详情，方便开发调试。

> 注意：开启日志聚集功能，需要重新启动NodeManager 、ResourceManager和HistoryManager。

开启日志聚集功能具体步骤如下：

1. 配置yarn-site.xml

   ```xml
   <!-- 日志聚集功能使能 -->
   <property>
   <name>yarn.log-aggregation-enable</name>
   <value>true</value>
   </property>
   
   <!-- 日志保留时间设置7天 -->
   <property>
   <name>yarn.log-aggregation.retain-seconds</name>
   <value>604800</value>
   </property>
   ```

2. 关闭NodeManager 、ResourceManager和HistoryServer
   `yarn-daemon.sh stop resourcemanager`
   `yarn-daemon.sh stop nodemanager`
   `mr-jobhistory-daemon.sh stop historyserver`

3. 启动NodeManager 、ResourceManager和HistoryServer

4. 删除HDFS上已经存在的输出文件
   `hdfs dfs -rm -R /user/csy/output`

5. 执行WordCount程序
   `hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/csy/input /user/csy/output`

6. 查看日志，如图2-37，2-38，2-39所示（http://hadoop101:19888/jobhistory）



#### 2.2.5 配置文件说明

Hadoop配置文件分两类：默认配置文件和自定义配置文件，只有用户想修改某一默认配置值时，才需要修改自定义配置文件，更改相应属性值。

1. 默认配置文件

   | 要获取的默认文件     | 文件存放的Hadoop的jar包中的位置                           |
   | -------------------- | --------------------------------------------------------- |
   | [core-default.xml]   | hadoop-common-2.7.2.jar/core.default.xml                  |
   | [hdfs-default.xml]   | hadoop-hdfs-2.7.2.jar/hdfs-default.xml                    |
   | [yarn-default.xml]   | hadoop-yarn-common-2.7.2.jar/yarn-default.xml             |
   | [mapred-default.xml] | hadoop-mapreduce-client-core-2.7.2.jar/mapred-default.xml |

2. 自定义配置文件 

   **core-site.xml、hdfs-site.xml、yarn-site.xml、mapred-site.xml**四个配置文件存放在$HADOOP_HOME/etc/hadoop这个路径上，用户可以根据项目需求重新进行修改配置。



### 2.3 完全分布式运行模式（开发重点）

分析：

1. 准备3台或以上客户机（这里为了方便入门就关闭防火墙、使用静态IP、主机命名好区分）
2. 安装JDK
3. 配置环境变量
4. 安装Hadoop
5. 配置环境变量
6. 配置集群
7. 单点启动
8. 配置ssh
9. 群起并测试集群

#### 2.3.1 虚拟机准备

见前面

#### 2.3.2 编写集群分发脚本xsync

1. scp（secure copy）安全拷贝

   （1）scp定义：

   ​	scp可以实现服务器与服务器之间的数据拷贝。（from server1 to server2）

   （2）基本语法

   ```shell
   scp    -r          $pdir/$fname              $user@hadoop$host:$pdir/$fname
   命令   递归       要拷贝的文件路径/名称    		目的用户@主机:目的路径/名称
   ```

   （3）用法示例

   ```shell
   #在hadoop101上，将hadoop101中/opt/module目录下的软件拷贝到hadoop102
   scp -r /opt/module root@hadoop102:/opt/module
   
   #在hadoop103上，将hadoop101服务器上的/opt/module目录下的软件拷贝到hadoop103上
   scp -r csy@hadoop101:/opt/module root@hadoop103:/opt/module
   
   #在hadoop103上操作将hadoop101中/opt/module目录下的软件拷贝到hadoop104上
   scp -r csy@hadoop101:/opt/module root@hadoop104:/opt/module
   ```

   > 拷贝过来的/opt/module目录，别忘了在hadoop102、hadoop103、hadoop104上修改所有文件的，所有者和所有者组。sudo chown csy:csy -R /opt/module

2. rsync 远程同步工具

   rsync主要用于备份和镜像。具有速度快、避免复制相同内容和支持符号链接的优点。

   rsync和scp区别：用rsync做文件的复制要比scp的速度快，rsync只对差异文件做更新。scp是把所有文件都复制过去。

   （1）基本语法

   ```shell
   rsync    -av       $pdir/$fname      $user@hadoop$host:$pdir/$fname
   命令	   选项参数  要拷贝的文件路径/名称     目的用户@主机:目的路径/名称
   ```

   选项参数说明

   | 选项 | 功能         |
   | ---- | ------------ |
   | -a   | 归档拷贝     |
   | -v   | 显示复制过程 |

   （2）案例实操

   ​	（a）把hadoop101机器上的/opt/software目录同步到hadoop102服务器的root用户下的/opt/目录

   ​		`rsync -av /opt/software/hadoop102:/opt/software`

3. xsync集群分发脚本

   （1）需求：复制文件到所有节点的相同目录下
   （2）需求分析：
   	（a）rsync命令原始拷贝：
   		`rsync -av /opt/module root@hadoop103:/opt`
   	（b）期望脚本：
   		`xsync 需要同步的文件名称`
   （3）脚本实现
   	（a）在/etc/profile 中配置环境变量 `exprot PATH=$PATH:/home/csy/bin`，然后将文件发送到每个集			群，source一下
   	（b）在/home/csy目录下创建bin目录，并在bin目录下xsync创建文件，文件内容如下：

   ```shell
   #!/bin/bash
   #1 获取输入参数个数，如果没有参数，直接退出
   pcount=$#
   if ((pcount==0)); then
   echo no args;
   exit;
   fi
   
   #2 获取文件名称
   p1=$1
   fname=`basename $p1`
   echo fname=$fname
   
   #3 获取上级目录到绝对路径
   pdir=`cd -P $(dirname $p1); pwd`
   echo pdir=$pdir
   
   #4 获取当前用户名称
   user=`whoami`
   
   #5 循环
   for((host=103; host<105; host++)); do
           echo ------------------- hadoop$host --------------
           rsync -av $pdir/$fname $user@hadoop$host:$pdir
   done
   ```

   ​	（c）修改脚本 xsync的执行权限
   ​		`chmod 777 xsync`
   ​	（d）调用脚本形式：`xsync [filename]`
   ​		`xsync /home/csy/bin`

> 注意在脚本的循环部分，for循环的条件跟你个人的节点ip来配置



#### 2.3.3 集群配置

1. 集群部署规划

   |      | hadoop102              | hadoop103                        | hadoop104                       |
   | ---- | ---------------------- | -------------------------------- | ------------------------------- |
   | HDFS | NameNode<br />DataNode | DataNode                         | SecondaryNameNode<br />DataNode |
   | YARN | NodeManager            | ResourceManager<br />NodeManager | NodeManager                     |

   > 将NameNode、ResourceManager、SecondaryNameNode放在不同的节点上，避免单节点压力过大
   >
   > 节点可以随自己的喜好规划，与我不一致亦可。
   >
   > 此处记住NameNode与ResourceManager所在的节点名称，后面需要分别在这两个节点上来群起集群

2. 配置集群:star:
   （1）核心配置文本

   ​	配置core-site.xml

   ```xml
   <!-- 指定HDFS中NameNode的地址 -->
   <property>
   		<name>fs.defaultFS</name>
         <value>hdfs://hadoop102:9000</value>
   </property>
   
   <!-- 指定Hadoop运行时产生文件的存储目录 -->
   <property>
   		<name>hadoop.tmp.dir</name>
   		<value>/opt/module/hadoop-2.7.2/data/tmp</value>
   </property>
   ```

   （2）HDFS配置文件

   ​	配置hadoop-env.sh
   ​	`export JAVA_HOME=/opt/module/jdk1.8.0_144`
   ​	配置hdfs-site.xml

   ```xml
   <property>
   		<name>dfs.replication</name>
   		<value>3</value>
   </property>
   <!-- 指定Hadoop辅助名称节点主机配置 -->
   <property>
         <name>dfs.namenode.secondary.http-address</name>
         <value>hadoop104:50090</value>
   </property>
   ```

   ​	（3）YARN配置文件
   ​		配置yarn-env.sh
   ​		`export JAVA_HOME=/opt/module/jdk1.8.0_144`
   ​		配置yarn-site.xml

   ```xml
   <!-- Reducer获取数据的方式 -->
   <property>
   		<name>yarn.nodemanager.aux-services</name>
   		<value>mapreduce_shuffle</value>
   </property>
   
   <!-- 指定YARN的ResourceManager的地址 -->
   <property>
   		<name>yarn.resourcemanager.hostname</name>
   		<value>hadoop103</value>
   </property>
   ```

   ​	（4）MapReduce
   ​		配置mapred-env.sh
   ​		`export JAVA_HOME=/opt/module/jdk1.8.0_144`
   ​		配置mapred-site.xml
   ​		`cp mapred-site.xml.template mapred-site.xml`:arrow_right:

   ​		`vi mapred-site.xml`

   ​		在该文件中增加如下配置

   ```xml
   <!-- 指定MR运行在Yarn上 -->
   <property>
   		<name>mapreduce.framework.name</name>
   		<value>yarn</value>
   </property>
   ```

3. 在集群上分发配置好的Hadoop配置文件
   `xsync /opt/module/hadoop-2.7.2/`

4. 到其他的节点上查看文件是否上发过去



#### 2.3.4 集群单点启动

1. 如果集群是第一次启动，需要格式化NameNode
   `hdfs namenode -format`
2. 在hadoop102上启动NameNode
   `hadoop-daemon.sh start namenode`
3. 使用`jps`查看节点服务启动情况
4. 在hadoop102、hadoop103以及hadoop104上分别启动DataNode
   `hadoop-daemon.sh start datanode`

> 逐个启动？是不是有点呆？:sweat_smile:

> 如果遇到NameNode起不来的问题，可以清除core-site.xml所配置的保存NameNode数据的目录，然后重新格式化。例如本文中配置的路径是/opt/module/hadoop-2.7.2/data，可以rm -rf删除此目录后重新格式化（`hdfs namenode -format`）



#### 2.3.5 SSH无密码登录配置

1. 配置SSH
   （1）基本语法 `ssh [ip or hostname]`
   （2）ssh连接时出现Host key verification failed的解决方法
   （3）解决方案如下，输入yes

2. 无密钥配置
   （1）免密登录原理

   ![1563526321744](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563526321744.png)
   （2）生成公钥和私钥`ssh-keygen -t rsa`，三次回车看到突然就生成成功。
   （3）将公钥拷贝到要免密登录的目标机器上

   ```shell
   ssh-copy-id hadoop102
   ssh-copy-id hadoop103
   ssh-copy-id hadoop104
   ```

   > 还需要在hadoop102上采用root账号，配置一下无密登录到hadoop102、hadoop103、hadoop104
   > 还需要在hadoop103上采用csy账号配置一下无密登录到hadoop102、hadoop103、hadoop104服务器上。

3.  *.ssh*文件夹下（*~/.ssh*）的文件功能解释

   | known_hosts | 记录ssh访问过计算机的公钥（public key） |
   | ----------- | --------------------------------------- |
   | id_rsa      | 生成私钥                                |
   | id_rsa.pub  | 生成的公钥                              |
   | authorized  | 存放授权过得无密登录服务器公钥          |



#### 2.3.6 群起集群

1. 配置slaves
   `vim /opt/module/hadoop-2.7.2/etc/hadoop/slaves` 

   ```shell
   hadoop102
   hadoop103
   hadoop104
   ```

   > 此文件中添加的内容不允许有空格，也不允许有空行

   同步所有节点配置文件`xsync slaves`

2. 启动集群
   （1）如果集群是第一次启动，需要格式化NameNode（注意格式化之前，一定要先停止上次启动的所有namenode和datanode进程，然后再删除data和log数据）
   `hdfs namenode -format`

   （2）启动HDFS
   	到core-site.xml中配置的节点去执行脚本（NameNode）：`start-dfs.sh`

   （3）启动YARN
   	到yarn-site.xml中配置的节点去执行脚本（ResourceManager）：`start-yarn.sh`

   （4）Web端查看SecondaryNameNode
   	 浏览器中输入：http://hadoop104:50090/status.html

3. 集群基本测试
   （1）上传文件到集群
   `hdfs dfs -mkdir -p /user/csy/input`  :arrow_right: 

   上传小文件
   `hdfs dfs -put LICENSE.txt`
   上传大文件
   `hadoop fs -put /opt/software/hadoop-2.7.2.tar.gz /user/csy/input`

   ![1563532114075](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563532114075.png)

   （2）上传文件后查看文件存放的位置
   	（a）查看HDFS在磁盘存储文件内容
   		`/opt/module/hadoop-2.7.2/data/tmp/dfs/data/current/BP-938951106-192.168.10.107-1495462844069/current/finalized/subdir0/subdir0`
   	（b）查看HDFS在磁盘存储文件内容
   		`cat blk_1073741825`
   ![1563532785626](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563532785626.png)
   （3）拼接
   	`cat blk_1073741825>>tmp.file`
   	`cat blk_1073741827>>tmp.file`
   	`tar -zxvf tmp.file`

   （4）下载（将文件下载到当前目录）
   	`hadoop fs -get /user/csy/input/hadoop-2.7.2.tar.gz ./`



#### 2.3.7 集群启动/停止方式

1. 各个服务逐一启动/停止
   - 分别启动/停止HDFS组件：
     `hadoop-daemon.sh start / stop namenode / datanode / secondarynamenode`
   - 启动/停止YARN
     `yarn-daemon.sh start / stop  resourcemanager / nodemanager`
2. 各个模块分开启动/停止（需提前配置好ssh并各个节点互相拥有秘钥）
   - 整体启动/停止HDFS：`start-dfs.sh` `stop-dfs.sh`
   - 整体启动/停止YARN：`start-yarn.sh` `stop-yarn.sh`



#### 2.3.8 集群时间同步

时间同步方式：找一个机器，作为时间服务器，所有的机器与这台集群时间进行定时的同步，比如，每隔十分钟，同步一次时间。

![1563535056575](C:\Users\13643\AppData\Roaming\Typora\typora-user-images\1563535056575.png)

配置时间同步具体实操：

1. 时间服务器配置（必须是root用户）

   - 检查ntp是否安装 `rpm -qa | grep ntp`

   - 修改ntp配置文件 `vim /etc/ntp.conf`
     修改内容如下
     （a）修改1（授权192.168.1.0-192.168.1.255网段上的所有机器可以从这台机器上查询和同步时间）**#**restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap去掉注释:arrow_right:

        restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap

     （b）修改2（集群在局域网中，不使用其他互联网上的时间）

     ```shell
     server 0.centos.pool.ntp.org iburst
     server 1.centos.pool.ntp.org iburst
     server 2.centos.pool.ntp.org iburst
     server 3.centos.pool.ntp.org iburst
     #改为
     #server 0.centos.pool.ntp.org iburst
     #server 1.centos.pool.ntp.org iburst
     #server 2.centos.pool.ntp.org iburst
     #server 3.centos.pool.ntp.org iburst
     ```

     （c）添加3（当该节点丢失网络连接，依然可以采用本地时间作为时间服务器为集群中的其他节点提供时间同步）

     ```shell
     server 127.127.1.0
     fudge 127.127.1.0 stratum 10
     ```

   - 修改*/etc/sysconfig/ntpd*文件
     `vim /etc/sysconfig/ntpd`:arrow_right: 添加 `SYNC_HWCLOCK=yes`

   - 重新启动ntpd服务
     `service ntpd status` :arrow_right: `service ntpd start`

   - 设置开机自启动
     `chkconfig ntpd on`

2. 其他机器配置（必须是root用户）

   - 在其他机器配置10分钟与时间服务器同步一次
     `crontab -e`
   - 修改任意机器时间
     `*/10 * * * * /usr/sbin/ntpdate hadoop102`
   - 十分钟后查看机器是否与时间服务器同步
     `date`

   > 测试时可以将十分钟改为一分钟



