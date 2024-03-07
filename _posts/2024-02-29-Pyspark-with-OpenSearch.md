---
layout: post
title:  "Pyspark with OpenSearch"
summary: "Pyspark with OpenSearch"
author: seunghwan
date: '2024-02-29 00:00:00 +0530'
category: ['Apache Spark', 'Pyspark', 'OpenSearch']
tags: Pyspark
usemathjax: false
permalink: /blog/Pyspark-with-OpenSearch/
---
## 환경

- Mac1
- OpenSearch version: 2.12.0
- pip3 version: python 3.12
- python3 version: python 3.12.2
- opensearch-py version: 2.4.2

*version 확인 방법

```bash
# OpenSearch version: 2.12.0
# Mac1에서 brew install opensearch를 통해서 OpenSearch를 다운로드 받았습니다.
$ brew info opensearch

# pip3 version: python 3.12
$ pip3 -V

# python3 version: python 3.12.2
$ python3 -V

# opensearch-py version: 2.4.2
$ pip3 list
```

## Mac에서 OpenSearch 설치하기

1. OpenSearch에서 데이터 가져오는 것은 파이썬 라이브러리 중 opensearchpy를 사용했습니다.
    1. 설치 방법
        1. pip3를 통해서 설치했습니다.
            
            ```bash
            $ pip3 install opensearch-py
            ```

## OpenSearch에 데이터 넣기

[목표]

- bank account 데이터를 다운로드 받아 넣습니다.

[방법]

1. wget을 통해 account 데이터를 다운로드 받아 압축을 풀어줍니다.
    
    ```bash
    $ wget https://download.elastic.co/demos/kibana/gettingstarted/accounts.zip
    $ unzip accounts.zip
    ```
    
2. 다운로드 받은 파일은 opensearch에 넣어줍니다.
    
    ```bash
    # curl을 사용해줍니다.
    # bash 디렉토리가 account.json과 같은 디렉토리에 있도록 해줍니다. 
    $ curl -XPOST \
        -H 'Content-Type: application/x-ndjson' \
        'http://localhost:9200/bank/_bulk?pretty' \
        --data-binary "@accounts.json"
    ```
    
3. 데이터가 잘 넣어졌는지 확인합니다.
    
    ```bash
    $ curl -XGET 'http://localhost:9200/bank/_search?pretty&size=0'
    ```

## OpenSearch에서 데이터 가져오기

1. OpenSearch에서 데이터 가져오는 것은 파이썬 라이브러리 중 opensearchpy를 사용했습니다.
    1. 설치 방법
        1. pip3를 통해서 설치했습니다.
            
            ```bash
            $ pip3 install opensearch-py
            ```
            
    2. 코드
    
    ```python
    from opensearchpy import OpenSearch
    
    # 만약 OpenSearch 계정 정보를 수정하지 않았으면 admin / admin으로 설정하면 됩니다.
    user_id = 'admin'
    passwd = 'admin'
    host = 'localhost'
    port = '443' # 대부분 443을 사용한다.
    
    hosts = [{'host':host, 'port':port}]
    auth = ('admin','admin')
    
    es = OpenSearch(
        hosts=host,
        http_compress = True,
        http_auth=auth,
        use_ssl = False,
        ssl_assert_hostname = False,
        ssl_show_warn = False
        )
       
    # 연결이 되었는지 확인합니다.
    es.ping()
    ```
    
2. opensearchpy search()를 사용합니다.
    
    ```bash
    # 기본적으로 size 값은 10개이기 때문에, 10개 이상의 값을 구할 때는 body에 size 값을 줘야합니다.
    es.search(
        index='bank',
        body={
            'size': 10000,
            'query' : {
                'match_all' : {}
            }
        }
    )
    ```

## python dict 객체를 DataFrame으로 변경하기

- dict 객체는 vl 객체에 담겨진것을 가정으로 진행하겠습니다.
- pyspark를 사용합니다.

1. pyspark와 SparkSession를 불러옵니다.
    
    ```python
    import pyspark
    from pyspark.sql import SparkSession
    ```
    
2. SparkSession 생성 후 spark createDataFrame() 함수를 통해 DataFrame을 생성해줍니다.
    
    ```python
    spark = SparkSession.builder.appName('sparkdf').getOrCreate() 
    
    # DataFrame을 생성해줍니다.
    dataframe = spark.createDataFrame(hits_with_source_only) 
    
    # 만들어진 DataFrame을 확인합니다.
    dataframe.show()
    ```
