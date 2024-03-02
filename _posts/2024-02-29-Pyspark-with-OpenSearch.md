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