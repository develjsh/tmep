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