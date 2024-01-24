---
layout: post
title:  "Setting up Big Data Analysis Environment"
summary: "Learn how to set up big data analysis environment"
author: seunghwan
date: '2024-01-22 00:00:00 +0530'
category: ['big_data', 'hadoop', 'spark']
tags: big_data
#thumbnail: /assets/img/posts/code.jpg
usemathjax: false
permalink: /blog/adding-categories-tags-in-posts/
---
# Setting up Big Data Analysis Environment

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

# 3강. Hadoop 설치 및 환경설정

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