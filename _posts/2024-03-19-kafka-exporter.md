---
layout: post
title:  "kafka-exporter"
summary: "What is kafka-exporter?"
author: seunghwan
date: '2024-03-19 00:00:00 +0530'
category: ['kafka-exporter']
tags: kafka-exporter
usemathjax: false
permalink: /blog/kafka-exporter/
---

## 설치 및 실행

### Mac m2

1. https://github.com/danielqsj/kafka_exporter/releases 을 통해서 kafka를 설치 후 압축 해제합니다.
    1. 작성자는 kafka_exporter-1.7.0.darwin-arm64.tar.gz를 설치했습니다.
2. 압축 해제한 파일을 사용할 파일로 이동합니다.
3. kafka_exporter를 아래 명령어로 실행합니다.
    
    ```bash
    $ ./kafka_exporter --kafka.server=<KAFKA_HOST>:<KAFKA_PORT> --web.listen-address=<EXPORTER_HOST>:<EXPORTER_PORT>
    ```
    
    - EXPORTER_HOST: 사용하실 kafka_exporter host를 설정합니다.
    - EXPORTER_PORT: 사용하실 kafka_exporter port를 설정합니다.
4. 실행되면 자신이 설정한 host와 port로 들어가서 kafka metric 정보를 확인할 수 있습니다.
    예시: http://localhost:9099/metrics
    