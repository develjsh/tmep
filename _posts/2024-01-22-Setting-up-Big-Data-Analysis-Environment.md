---
layout: post
title:  "Setting up Big Data Analysis Environment"
summary: "Learn how to set up big data analysis environment"
author: seunghwan
date: '2024-01-22 00:00:00 +0530'
category: ['big-data', 'hadoop', 'spark']
tags: big_data
#thumbnail: /assets/img/posts/code.jpg
usemathjax: false
permalink: /blog/big-data/setting-up-big-data-analysis-environment/
---
## Setting up Big Data Analysis Environment

## 0강. 고가용성 Hadoop 클러스터 환경 구축합니다.
- 총 5개의(nn1, nn2, dn1, dn2, dn3) vm 이 필요합니다.

## 1강. GCP VM 인스턴스 배포

1. Compute Engine 을 클릭 후 인스턴스 만들기를 클릭합니다.

2. 각 원하는 구성을 선택해줍니다. 아래 내용은 참고 내용 입니다.
    - 리전: 서울
    - 머신 구성: E2
    - 머신 유형: e2-standard-4(vCPU 4개, 코어 2개, 메모리 16GB)
    - 엑섹스 범위: 기본 엑세스 허용
    - 방화벽: HTTP 트래픽 허용, HTTPS 트래픽 허용

3. ssh-keygen 을 통해 ssh key 를 생성해 줍니다.
    - $ ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]
        - [타입명] : key 생성 type을 선택하는 옵션입니다.
        - f [파일명] : 생성할 key 파일의 이름을 설정하는 옵션입니다.
        - C [내용] : 사용자에 대한 주석을 입력합니다.
        - b [숫자] : type의 bytes를 설정합니다. rsa 암호화 방식은 기본 값이 2048이며, 4096으로 더 안전한 키를 생성할 수도 있습니다.
    - ssh key 가 public(.pub) 과 private 이 생성됩니다.

4. 생성된 ssh key 를 GCP 콘솔 서버에 등록합니다.
    - Compute Engine 에서 ‘VM 인스턴스’ 있는 사이드 바에 메타데이터가 존재합니다. 메타데이터에 들어간 후, SSH 키 태그를 누릅니다.
    - 추가를 합니다.

5. ~/.ssh 폴더에 config 파일을 만들어 ssh 연결 설정을 해줍니다.
    ```yml
    ---
    Host nn1
        HostName 10.10.171.101
        User jay
        IdentityFile ~/.ssh/rsa_big_data
    ---
    ```
    **Troubleshooting
    VM 인스턴스를 정지 후 새로 시작하면 외부 IP 주소가 바뀌기 때문에 재실행 시 config 설정 값도 수정해줘야 합니다.

6. ssh nn1 을 터미널에 입력 후 실행해 Server 에 연결 되는지 확인합니다.


## 2강. Java 설치 및 환경설정

1. Ubuntu 20.04 apt-get update, upgrade, dist-upgrade 를 통해서 최신 version 으로 설치합니다.

2. 프로젝트 진행 중에 필요한 라이브러리를 설치 합니다.

    ```bash
    > sudo apt-get install -y vim wget ssh openssh-* net-tools
    ```

3. Java 를 설치해줍니다.

    ```bash
    > sudo apt-get install -y openjdk-8-jdk
    ```

4. 현재 java 설치 위치를 확인합니다.
    
    ```bash
    > sudo find / -name java-8-openjdk-amd64 2>/dev/null
    -> /usr/share/gdb/auto-load/usr/lib/jvm/java-8-openjdk-amd64
    -> /usr/lib/jvm/java-8-openjdk-amd64
    ```
    
5. 횐경변수를 설정해줍니다.
    1. > sudo vim /etc/environment
        
        ```bash
        PATH="~~~~~:/usr/lib/jvm/java-8-openjdk-amd64/bin"
        
        JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
        ```
        
    2. > source /etc/environment

6. > sudo echo export ‘JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64’ >> ~/.bashrc
    
    **Troubleshooting
    복사 붙여넣기 하게 되면 ‘ 값이 잘못 들어갈 수 있어 한 번 더 확인해봐야합니다.
    
7. > env | grep JAVA 를 통해서 JAVA_HOME 이 export 되고 있는지 확인할 수 있습니다.


## 3강. Hadoop 설치 및 환경설정

1. Hadoop 설치 해줍니다.
    
    ```yaml
    > sudo wget https://dlcdn.apache.org/hadoop/common/hadoop-3.2.4/hadoop-3.2.4.tar.gz
    
    > sudo tar -zxvf hadoop-3.2.4.tar.gz -C /usr/local
    
    > sudo mv /usr/local/hadoop-3.2.4 /usr/local/hadoop
    ```
    
2. /etc/environment 에설정을 추가해줍니다.
    
    ```bash
    PATH="~~~:/usr/local/hadoop/bin:/usr/local/hadoop/sbin"
    
    HADOOP_HOME="/usr/local/hadoop"
    ```
    
3. 추가 설정을 해줍니다.
    
    ```bash
    //Ubuntu
    > sudo echo export 'HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc
    > sudo echo export 'HADOOP_COMMON_HOME=$HADOOP_HOME' >> ~/.bashrc
    > sudo echo export 'HADOOP_HDFS_HOME=$HADOOP_HOME' >> ~/.bashrc
    > sudo echo export 'HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
    > sudo echo export 'YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
    > sudo echo export 'HADOOP_YARN_HOME=$HADOOP_HOME' >> ~/.bashrc
    > sudo echo export 'HADOOP_MAPRED_HOME=$HADOOP_HOME' >> ~/.bashrc
    ```
    
4. hdfs-site.xml 파일을 편집 해줍니다.
    - HDFS에서 사용할 환경 정보를 설정하는 파일입니다. hdfs-site.xml 에 설정 값이 없을 경우 hdfs-default.xml 을 기본으로 사용합니다.
    - 경로는 아래와 같습니다.
        > $HADOOP_HOME/etc/hadoop/hdfs-site.xml
        
    ```bash
    <configuration>
        <!-- configuration hadoop -->
        <property>
            <name>dfs.replication</name>
            <value>2</value>
        </property>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>/usr/local/hadoop/data/nameNode</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>/usr/local/hadoop/data/dataNode</value>
        </property>
        <property>
            <name>dfs.journalnode.edits.dir</name>
            <value>/usr/local/hadoop/data/dfs/journalnode</value>
        </property>
        <property>
            <name>dfs.nameservices</name>
            <value>my-hadoop-cluster</value>
        </property>
        <property>
            <name>dfs.ha.namenodes.my-hadoop-cluster</name>
            <value>namenode1,namenode2</value>
        </property>
        <property>
            <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode1</name>
            <value>nn1:8020</value>
        </property>
        <property>
            <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode2</name>
            <value>nn2:8020</value>
        </property>
        <property>
            <name>dfs.namenode.http-address.my-hadoop-cluster.namenode1</name>
            <value>nn1:50070</value>
        </property>
        <property>
            <name>dfs.namenode.http-address.my-hadoop-cluster.namenode2</name>
            <value>nn2:50070</value>
        </property>
        <property>
            <name>dfs.namenode.shared.edits.dir</name>
            <value>qjournal://nn1:8485;nn2:8485;dn1:8485/my-hadoop-cluster</value>
        </property>
        <property>
            <name>dfs.client.failover.proxy.provider.my-hadoop-cluster</name>
            <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
        </property>
        <property>
            <name>dfs.ha.fencing.methods</name>
            <value>shell(/bin/true)</value>
        </property>
        <property>
            <name>dfs.ha.fencing.ssh.private-key-files</name>
            <value>/home/ubuntu/.ssh/id_rsa</value>
        </property>
        <property> 
            <name>dfs.ha.automatic-failover.enabled</name>
            <value>true</value>
        </property>
        <property>
            <name>dfs.name.dir</name>
            <value>/usr/local/hadoop/data/name</value>
        </property>
        <property>
            <name>dfs.data.dir</name>
            <value>/usr/local/hadoop/data/data</value>
        </property>
    </configuration>
    
    <configuration>
    	<property>
    		<name>fs.default.name</name>
    		<value>hdfs://nn1:9000</value>
    	</property>
    	<property>
    		<name>fs.defaultFS</name>
    		<value>hdfs://my-hadoop-cluster</value>
    	</property>
    	<property>
    		<name>ha.zookeeper.quorum</name>
    		<value>nn1:2181,nn2:2181,dn1:2181</value>
    	</property>
    </configuration>
    
    ```
    
    - hdfs-site.xml 속성
        - dfs.replication : HDFS 파일 블록 복제 개수 지정합니다.
        - dfs.namenode.name.dir : NameNode에서 관리할 데이터 디렉토리 경로 지정합니다.
            - namenode: Hadoop HDFS 의 Master daemon 입니다.
        - dfs.datanode.data.dir : DataNode에서 관리할 데이터 디렉토리 경로 지정합니다.
        - dfs.journalnode.edits.dir : JournalNode는 NameNode의 동기화 상태를 유지한다. 특정 시점에 구성된 fsimage snapshot 이후로 발생된 변경 사항을 editlog라 하며, 해당 데이터의 저장 위치를 설정합니다.
        - dfs.nameservices : Hadoop 클러스터의 네임서비스 이름을 지정합니다.
        - dfs.ha.namenodes.my-hadoop-cluster : Hadoop 클러스터 네임서비스의 NameNode 이름을 지정합니다.( “,”콤마로 구분하여 기재한다.)
        - dfs.namenode.rpc-address.my-hadoop-cluster.namenode1 : 클러스터 네임서비스에 포함되는 NameNode 끼리 RPC 통신을 위해 NameNode의 통신 주소를 지정합니다.(여기서는 8020포트 사용)
        - dfs.namenode.rpc-address.my-hadoop-cluster.namenode2 : 클러스터 네임서비스에 포함되는 NameNode 끼리 RPC 통신을 위해 NameNode의 통신 주소를 지정합니다.(여기서는 8020포트 사용)
        - dfs.namenode.http-address.my-hadoop-cluster.namenode1 : NameNode1(nn1)의 WEB UI 접속 주소를 지정합니다.(여기서는 50070포트 사용)
        - dfs.namenode.http-address.my-hadoop-cluster.namenode2 : NameNode2(nn2)의 WEB UI 접속 주소를 지정합니다.(여기서는 50070포트 사용)
        - dfs.namenode.shared.edits.dir : NameNoderk editlog를 쓰고/읽을 JournalNode URL 입니다. Zookeeper가 설치된 서버와 동일하게 JournalNode를 설정하면 됩니다. (예 : qjournal://nn1:8485;nn2:8485;dn1:8485/my-hadoop-cluster)
        - dfs.client.failover.proxy.provider.my-hadoop-cluster : HDFS 클라이언트가 Active NameNode에 접근할 때 사용하는 Java class 를 지정합니다.
        - dfs.ha.fencing.methods : Favilover 상황에서 기존 Active NameNode를 차단할 때 사용하는 방법을 기재합니다. (예 : sshfence 그러나 여기서는 shell(/bin/true)를 이용합니다.)
        - dfs.ha.fencing.ssh.private-key-files : ha.fencing.method를 sshfence로 지정하였을 경우, ssh를 경유하여 기존 Active NameNode를 죽이는데. 이 때, passpharase를 통과하기 위해 SSH Private Key File을 지정해야합니다.
        - dfs.ha.automatic-failover.enabled : 장애 복구를 자동으로 할 지에 대한 여부를 지정합니다.
5. core-site.xml 파일을 편집합니다.
    - Hadoop 시스템 설정 파일이다. 네트워크 튜닝, I/O 튜닝, 파일 시스템 튜닝, 압축 설정 등 Hadoop 시스템 설정을 할 수 있습니다. HDFS와 MapReduce에서 공통적으로 사용할 환경 정보를 입력할 수 있습니다. core-site.xml 설정 값이 없으면, core-default.xml 을 기본으로 사용합니다.
    
    ```bash
    <configuration>
    	<property>
    		<name>fs.default.name</name>
    		<value>hdfs://nn1:9000</value>
    	</property>
    	<property>
    		<name>fs.defaultFS</name>
    		<value>hdfs://my-hadoop-cluster</value>
    	</property>
    	<property>
    		<name>ha.zookeeper.quorum</name>
    		<value>nn1:2181,nn2:2181,dn1:2181</value>
    	</property>
    </configuration>
    ```
    
    - fs.default.name : HDFS의 기본 통신 주소를 지정합니다.
    - fs.defaultFS : HDFS 기본 파일시스템 디렉토리를 지정합니다.
    - ha.zookeeper.quorum : Zookeeper가 설치되어 동작할 서버의 주소를 기재합니다.(여기서 포트는 2181)
6. yarn-site.xml 파일을 편집합니다.
    
    ```bash
    <configuration>
    	<!-- Site specific YARN configuration properties -->
    	<property>
    			<name>yarn.nodemanager.aux-services</name>
    			<value>mapreduce_shuffle</value>
    	</property>
    	<property>
    		<name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
    		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
    	</property>
    	<property>
    		<name>yarn.resourcemanager.hostname</name>
    		<value>nn1</value>
    	</property>
    	<property>
    		<name>yarn.nodemanager.vmem-check-enabled</name>
    		<value>false</value>
    	</property>
    </configuration>
    ```
    
7. mapred-site.xml 파일을 편집합니다.
    
    ```bash
    <configuration>
    	<property>
    		<name>mapreduce.framework.name</name>
    		<value>yarn</value>
    	</property>
    	<property>
    		<name>yarn.app.mapreduce.am.env</name>
    		<value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    	</property>
    	<property>
    		<name>mapreduce.map.env</name>
    		<value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    	</property>
    	<property>
    		<name>mapreduce.reduce.env</name>
    		<value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    	</property>
    </configuration>
    ```
    
8. [hadoop-env.sh](http://hadoop-env.sh) 파일을 편집합니다.
    
    ```xml
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    export HADOOP_HOME=/usr/local/hadoop
    ```
    
9. Hadoop workers 편집을 해줍니다.
    
    ```bash
    # Hadoop workers 편집
    sudo vim $HADOOP_HOME/etc/hadoop/workers
    
    # 아래 내용 수정 후 저장
    # localhost << 주석 처리 또는 제거
    dn1
    dn2
    dn3
    ```
    
10. Hadoop masters 편집을 해줍니다.
    
    ```xml
    # Hadoop masters 편집
    sudo vim $HADOOP_HOME/etc/hadoop/masters
    
    # 아래 내용 수정 후 저장
    nn1
    nn2
    ```


## 4강. Spark 설치 및 환경설정

1. Apache Spark 3.3.3 설치 및 압축 해제를 해줍니다.
    
    ```bash
    $ sudo wget https://dlcdn.apache.org/spark/spark-3.4.2/spark-3.4.2-bin-hadoop3.tgz
    
    $ sudo tar -xzvf spark-3.4.2-bin-hadoop3.tgz -C /usr/local
    
    $ sudo mv /usr/local/spark-3.4.2-bin-hadoop3 /usr/local/spark
    ```
    
2. Python3 설치 및 파이썬 라이브러리 설치 해줍니다.
    
    ```bash
    # Python 설치
    $ sudo apt-get install -y python3-pip
    # Python 버전 확인
    $ python3 -V
    #2024.01.25 기분 Python 3.8.10
    # PySpark 설치
    $ sudo pip3 install pyspark findspark
    ```
    
3. Spark 환경 변수 설정을 해줍니다.
    
    ```bash
    # Hadoop 시스템 환경변수 설정
    sudo vim /etc/environment
    
    # 아래 내용 추가 후 저장
    PATH 뒤에 ":/usr/local/spark/bin" 추가
    PATH 뒤에 ":/usr/local/spark/sbin" 추가
    SPARK_HOME="/usr/local/spark"
    
    # 시스템 환경변수 활성화
    $ source /etc/environment
    
    #  Spark 사용자 환경변수 설정
    $ echo 'export SPARK_HOME=/usr/local/spark' >> ~/.bashrc
    
    # 사용자 환경변수 활성화
    $ source ~/.bashrc
    ```
    
4. spark-env.sh 파일을 편집해줍니다.
    
    ```bash
    #터미널에서 입력해줍니다.
    $ cd $SPARK_HOME/conf
    $ ls 
    $ sudo cp spark-env.sh.template spark-env.sh
    $ sudo vi spark-env.sh
    
    #맨 아래로 이동합니다.
    export SPARK_HOME=/usr/local/spark
    export SPARK_CONF_DIR=/usr/local/spark/conf
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    export HADOOP_HOME=/usr/local/hadoop
    export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
    export SPARK_MASTER_WEBUI_PORT=18080
    ```
    
5. spark-defaults.conf 파일을 편집해줍니다.
    
    ```bash
    #터미널에서 입력해줍니다.
    $ sudo cp spark-defaults.conf.template spark-defaults.conf
    $ sudo vi spark-defaults.conf
    
    #맨 아래로 이동합니다.
    spark.master  yarn
    spark.eventLog.enabled  true
    spark.eventLog.dir  /usr/local/spark/logs
    #저장 후 나옵니다
    
    #터미널에서 입력해줍니다.
    $ sudo mkdir -p /usr/local/spark/logs
    $ sudo chown -R $USER:$USER /usr/local/spark/
    $ cd /usr/local/spark
    $ ls -al
    ```
    
6. workers.template 파일을 편집해줍니다.
    
    ```bash
    #터미널에서 입력해줍니다.
    $ cd /usr/local/spark/conf
    $ sudo cp workers.template workers
    $ vi workers
    
    #맨 아래로 이동해줍니다.
    #localhost 는 주석 처리해줍니다.
    #아래 내용을 추가해줍니다.
    dn1
    dn2
    dn3
    ```
    
7. 파이썬 환경 설정을 추가해줍니다
    
    ```bash
    $ sudo vi /etc/environment
    
    # 아래 내용 추가 후 저장
    PATH 뒤에 ":/usr/bin/python3" 추가
    
    # 시스템 환경변수 활성화
    $ source /etc/environment
    
    $ sudo echo 'export PYTHONPATH=/usr/bin/python3' >> ~/.bashrc
    $ sudo echo 'export PYSPARK_PYTHON=/usr/bin/python3' >> ~/.bashrc
    
    $ source ~/.bashrc
    ```

## 5강. Zookeeper 설치 및 환경 설정

1. 설치합니다.
    
    ```bash
    $ cd /install_dir
    $ sudo wget https://dlcdn.apache.org/zookeeper/zookeeper-3.8.3/apache-zookeeper-3.8.3-bin.tar.gz
    $ sudo tar -xzvf apache-zookeeper-3.8.3-bin.tar.gz -C /usr/local/zookeeper
    $ sudo vi /etc/environment
    
    # 아래 내용 추가 후 저장
    ZOOKEEPER_HOME="/usr/local/zookeeper"
    
    # 시스템 환경변수 활성화
    $ source /etc/environment
    $ sudo echo 'export ZOOKEEPER_HOME=/usr/local/zookeeper' >> ~/.bashrc
    $ source ~/.bashrc
    
    ```
    
2. zookeeper 환경 설정 파일을 설정해줍니다.
    
    ```bash
    $ cd $ZOOKEEPER_HOME
    $ sudo cp ./conf/zoo_sample.cfg ./conf/zoo.cfg
    $ sudo vi ./conf/zoo.cfg
    
    #기존에 존재하는 dataDir 은 주석 처리합니다.
    dataDir=/usr/local/zookeeper/data
    dataLogDir=/usr/local/zookeeper/logs
    
    #clientPort 아래에 추가해주고 저장합니다.
    maxClientCnsns=0
    maxSessionTimeout=180000
    server.1=nn1:2888:3888
    server.2=nn2:2888:3888
    server.3=dn1:2888:3888
    
    #터미널에서 입력해줍니다.
    $ sudo mkdir -p /usr/local/zookeeper/data
    $ sudo mkdir -p /usr/local/zookeeper/logs
    $ sudo chown -R $USER:$USER /usr/local/zookeeper
    $ sudo vi /usr/local/zookeeper/data/myid
    ```
    
3. Cluster 구성 후 Node 끼리 ssh 비밀번호 없이 연결할 수 있도록 설정합니다.
    
    ```bash
    #터미널에서 입력합니다.
    $ ssh-keygen -t rsa
    $ cat >> ~/.ssh/authorized_keys < ~/.ssh/id_rsa.pub
    $ ssh localhost
    ```


## 6강. AMI 생성 및 인스턴스 복제

1. VM 인스턴스 목록에서 SSH 옆에 점 3개를 누르면 ‘새머신 이미지 만들기’ 있습니다. 해당 목록을 클릭합니다.

2. 필요에 맞게 설정 후 만들기를 누르면 새머신 이미지가 만들어집니다.

3. 머신 이미지 창에서 작업을 클릭하면 ‘인스턴스 만들기’ 가 있습니다. 클릭 합니다.

4. 필요에 맞게 설정 후 만들기를 누르면 새머신이 만들어집니다. 총 5개를 만듭니다.
    **AWS 는 인스턴스를 여러개 만들수 있지만, GCP 는 안되는것 같습니다.
    
5. 5개의 머신이 통신할 수 있도록 보안그룹을 수정합니다.
    **GC 방화벽은 기본적으로 다 열려 있는 상태인지 따로 작업을 하지 않았습니다.


## 7강. SSH 및 호스트 이름 설정

1. ssh config 파일을 편집합니다.

    ```python
    #ssh config 파일을 편집합니다.
    $ vi ~/ssh/config

    Host nn1
        HostName ~
        User ~
        IdentifyFile <>

    Host nn2
        HostName ~
        User ~
        IdentifyFile ~

    Host dn1
        HostName ~
        User ~
        IdentifyFile ~

    Host dn2
        HostName ~
        User ~
        IdentifyFile ~

    Host dn3
        HostName ~
        User ~
        IdentifyFile ~
    ```
    
2. nn1 서버로 접속하여 /etc/hosts 파일을 변경해줍니다.
    
    ```bash
    #다른 인스턴스와 연결할 수 있도록 설정을 추가해줍니다.
    #여기서는 내부 IP 주소를 추가해줍니다.
    127.0.0.1 localhost
    #nn1
    10.178.0.2 nn1
    #nn2
    10.178.0.3 nn2
    #dn1
    10.178.0.4 dn1
    #dn2
    10.178.0.5 dn2
    #dn3
    10.178.0.6 dn3
    
    #다 입력 후 저장합니다.
    
    #host 이름이 내부 ip 여서 확인이 힘들기 때문에 host 이름을 변경해줍니다.
    #GCP 는 자동으로 host 이름이 내부 ip 가 아니라 이름으로 되어 있는거 같습니다.
    $ sudo hostnamectl set-hostname nn1
    
    ```
    
3. nn1 에 /etc/hosts 에 있는 설정 값들을 nn2, dn1, dn2. dn3 에 복사를 해줍니다.
    
    ```bash
    #CLI 로 한번에 복사해줍니다.
    cat /etc/hosts | ssh nn2 "sudo sh -c 'cat >/etc/hosts'"
    cat /etc/hosts | ssh dn1 "sudo sh -c 'cat >/etc/hosts'"
    cat /etc/hosts | ssh dn2 "sudo sh -c 'cat >/etc/hosts'"
    cat /etc/hosts | ssh dn3 "sudo sh -c 'cat >/etc/hosts'"
    
    ```
    
4. nn1 에 HADOOP_HOME 홈디렉토리 밑에 hdfs-site.xml 을 nn2 에 복사해줍니다.

    ```bash
    cat $HADOOP_HOME/etc/hadoop/hdfs-site.xml | ssh nn2 "sudo sh -c 'cat >$HADOOP_HOME/etc/hadoop/hdfs-site.xml'"

    ```

---

## 8강. Zookeeper 클러스터 실행

1. nn2 와 dn1 에 myid 파일을 편집해줍니다.
    
    ```bash
    #myid 파일을 열어줍니다.
    $ sudo vi /usr/local/zookeeper/data/myid
    
    #nn2 는 2, dn1 은 3으로 바꿔줍니다.
    ```
    
2. $ZOOKEEPER_HOME/bin/zookeeper.sh 라는  쉘 파일을 실행해줍니다.
    
    ```bash
    $ sudo $ZOOKEEPER_HOME/bin/zkServer.sh start
    ```
    
3. zookeeper 가 잘 실행되었는지 확인합니다.
    
    ```bash
    $ sudo $ZOOKEEPER_HOME/bin/zkServer.sh status
    
    #Leader 와 follower 를 확인할 수 있습니다.
    #Leader 선택은 랜덤입니다.
    ```
    
4. nn1 에서만 zkfc 라는 명령으를 통해 hdfs 를 초기화 해줍니다.
    
    ```bash
    $ hdfs zkfc -formatZK
    
    #초기화 되었는지 확인합니다.
    $ cd /usr/local/zookeeper
    $ ./bin/zkCli.sh
    
    #쉘이 실행되면 입력 할 수 있습니다
    ls /hadoop-ha
    #설정했던 하둡 클러스터 이름이 나오면 잘 실행된것입니다.
    
    #종료합니다.
    $ quit
    ```
    
    **Trouble
    
    - ERROR: Unable to create /usr/local/hadoop/logs. Aborting
        - Solution: sudo chmod 777 -R /usr/local/hadoop
5. journalnode 를 실행해줍니다.
    
    ```bash
    $ hdfs --daemon start journalnode
    
    #journalnode 가 실행되었는지 확인합니다.
    $ jps
    
    ```

## 9강. Hadoop & Yarn 클러스터 실행

1. nn1 서버에서 namenode 를 초기화해줍니다.
    
    ```bash
    #nn1 에 접속 후 hadfs bin 폴더로 이동 후(ex. /uar/local/hadoop/bin) namenode 를 초기화 해줍니다.
    $ hdfs namenode -format
    ```
    
2. namdenode 실행해줍니다.
    
    ```bash
    $ hdfs --daemon start namenode
    
    #jps 를 통해 실행되었는지 확인합니다.
    $ jps
    
    7313 Jps
    7334 NameNode
    5381 JournalNode
    
    ```
    
3. standby 로 실행될 namenode 인 nn2 에서도 namenode 를 초기화해줍니다.
    
    ```bash
    #nn2 에 접속 후 hadfs bin 폴더로 이동 후(ex. /uar/local/hadoop/bin) namenode 를 standby 로 실행해주니다.
    $ hdfs namenode -bootstrapStandby
    
    #nn1 에서 ~/sbin 폴더로 이동 후 start-dfs.sh 를 실행해 줍니다.
    $ start-dfs.sh
    
    #jps 로 확인합니다.
    $ jps
    
    #새롭게 DFSKFailoverController 가 생긴 것을 확인할 수 있습니다.
    
    #nn2 에서도 DFSKFailoverController 가 생긴 것을 확인할 수 있습니다.
    
    ```
    
4. nn1 에서 Yarn 을 실행해줍니다. 
    
    ```bash
    #~/hadoop/sbin 폴더에서 start-yarn.sh 을 실행해줍니다.
    $ start-yarn.sh
    
    #실행 확인
    $ jps
    
    # ResourceManager 가 실행된거 확인할 수 있습니다.
    #nn2 에는 ResourceManager가 없고 dn1, dn2, dn3 dp DataNode 가 실행된것을 확인할 수 있습니다.
    ```
    
    **Trouble
    - dn 서버들의 logs 폴더에 권한이 없어 실패가 발생했습니다.
    
    -> sudo chmod 777 -R /usr/local/hadoop
    
    - dn2: WARNING: /usr/local/hadoop/logs does not exist. Creating.
    
    dn1: nodemanager is running as process 7965.  Stop it first and ensure /tmp/hadoop-sh.jung2-nodemanager.pid file is empty before retry.
    
    dn3: WARNING: /usr/local/hadoop/logs does not exist. Creating.
    
    dn1 에서 7965 가 실행되고 있다고 나옵니다. 그 이유는 dn2 와 dn3 가 logs 폴더에 권한이 없어 실패할 때 dn1 은 권한이 있어 실행이 성공했었기 때문입니다.
    
5. job histry 를 실행시켜 줍니다.
    
    ```bash
    #nn1 에서 job history 를 실행해 줍니다.
    $ mapred --daemon start historyserver
    
    #jps 로 JobHistoryServer 가 실행되었는지 확인합니다.
    ```
    
6. nn1, nn2 에서 namenode 상태가 어떤지 확인합니다.
    
    ```bash
    $ hdfs haadmin -getServiceState namenode1
    ```
    
    **Trouble
    
    - $ hdfs haadmin -getServiceState namenode1 실행 했을 때 namenode1 의 결과가 active 가 아닌 standby 입니다.
        
        **[해결]**
        
        - nn1 터미널에서 $ hdfs haadmin -transitionToStandby namenode2 —forcemanual 를 입력하여 active를 standby 로 바꾸었습니다.
7. Hadoop 이 정상적으로 실행되는지 확인합니다.
    
    ```bash
    #nn1 서버에서만 실행합니다.
    $ hdfs dfs -mkdir /test
    $ hdfs dfs -ls /
    $ hdfs dfs -put /usr/local/hadoop/LICENSE.txt /test/
    $ hdfs dfs -ls /test/
    $ yarn jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-example-3.2.4.jar wordcount hdfs:///test/LICENSE.txt /test/output
    
    #결과 확인
    $ hdfs dfs -text /test/output/*
    
    setw -g mode-keys vi 
    bind -t vi-copy 'v' begin-selection
    bind -t vi-copy 'y' copy-selection
    bind C-c run "tmux save-buffer - | xclip -i -sel clipboard"
    bind C-v run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"
    
    ```

## 10강. Spark 클러스터 실행 및 PySpark 예제 실행

1. Spark 클러스터 실행
    
    ```bash
    #Spark 클러스터를 실행해줍니다.
    $ $SPARK_HOME/sbin/start-all.sh
    
    #지금까지 실행한게 실행되고 있는지 확인하기 위해 모든 서버에서 jps 를 입력합니다.
    #jps 입력후 나온 결과는 아래 사진과 같아야합니다.
    ```
    
    **Trouble
    
    - dn2, dn3 에 DataNode 가 실행되어 있지 않아 수동으로 실행해줬습니다.
        
        **정말 수동으로 DataNode 실행한게 작동하고 있는지 확인해봐야합니다.
        
2. nn1 에서 Spark wordcount 예제를 실행해봅니다.
    
    ```bash
    $ spark-submit --class org.apache.spark.examples.SparkPi \ #Java 에서 사용할 클래스 명을 설정해줍니다.
     --master yarn \ #master 를 yarn 으로 설정해줍니다.
    --deploy-mode cluster \ #애플리케이션을 동작시킬 모드를 설정해줍니다.
    --driver-memory 512m --executor 512m --executor-cores 1 \ #Resource 자원의 용량을 설정해줍니다.
    --$SPARK_HOME/examples/jars/spark-examples_2.12-3.4.2.jar 5 #해당 파일에 위치를 설정해줍니다.
    
    ```
    
3. 2 번 출력 결과를 확인해줍니다.
    
    *deploy mode 가 cluster 로 실행했기 때문에 출력 결과를 확인할 수 없습니다. 이유는 deploy mode 가 cluster 로 실행하면 yarn resource manager 가 해당 애플리케이션이 실행 가능한 worker node 를 찾아서 executor 드라이버가 실행하기 때문입니다. 실행 확인을 위해서는 cluster 가 아닌 client 로 실행해줘야 합니다. 그러면 해당 서버에 executor 가 실행이 됩니다.
    
4. PySpark 예제를 만들어서 실행해봅니다.
    
    ```bash
    $ vi pyspark_ex.py
    
    #아래 내용 입력 후 저장합니다.
    from pyspark import SparkContext, SparkConf
    
    conf = SparkConf()
    conf.setMaster("yarn")
    conf.setAppName("jsh")
    sc = SparkContext(conf=conf)
    
    print("="*100, "\n")
    print(sc)
    print("="*100, "\n")
    
    #spark-submit 으로 테스트 예제를 실행해줍니다.
    $ spark-submit --master yarn --deploy-mode cluster pyspark_ex.py
    
    #csv 파일을 다운받아 테스를 합니다.
    #csv 파일은 https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=9d00dd80-4a54-11eb-af9a-4b03f0a582d6 에서 원하는 데이터를 다운로드 받습니다.
    #현재 테스트는 KOBIS 박스오피스 영화정보 입니다.
    
    #scp 를 통해 파일은 nn1 서버로 전송합니다.
    $ scp ./box-office.csv nn1:~/
    
    #hdfs 로 파일은 이동해줍니다.
    $ hdfs dfs -put box-office.csv /test/
    
    #hdfs 에 들어갔는지 확인합니다.
    $ hdfs dfs -ls /test/
    
    #파이썬 예제를 만듭 후 저장하고 나옵니다.
    $ vi pyspark_ex2.py
    
    from pyspark.sql import SparkSession
    
    sc = SparkSession.builder \
    .master("yarn)\
    .appName("jsh")\
    getOrCreate()
    
    df = sc.read.scv("hdfs:///test/box-office.csv", header=True)
    
    df.show()
    
    #파이썬 예제를 실행합니다.
    $ spark-submit --deploy-mode client  pyspark_ex2.py
    
    ```
    
    **Trouble
    
    - df.show() 했을 때 한글이 깨져서 나옵니다.
        **[시도]**
        
        - $ spark-submit --deploy-mode client --conf spark.yarn.appMasterEnv.LANG=ko_KR.UTF-8 pyspark_ex2.py
            - 여전히 깨져서 나옵니다.
        - utf-8 설정 코드를 .py 파일에 추가해줍니다.
            
            ```bash
            import sys
            import codecs
            sys.stdout = codecs.getwriter('utf8')(sys.stdout)
            ```
            
            - TypeError: write() argument must be str, nto bytes
