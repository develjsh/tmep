---
layout: post
title:  "kafka-lag-exporter"
summary: "What is kafka-lag-exporter?"
author: seunghwan
date: '2024-03-20 00:00:00 +0530'
category: ['kafka-lag-exporter']
tags: kafka-lag-exporter
usemathjax: false
permalink: /blog/kafka-lag-exporter/
---

## 설치 및 실행

*여러 방법으로 실행하는 방법이 있는데 Java 방식으로 설치 및 실행했습니다.

1. https://github.com/seglo/kafka-lag-exporter/releases 페이지에서 zip 형태를 다운로드 받습니다. Source code는 example 파일 사용을 위해 다운로드 받습니다.
2. java를 설치하거나, java version이 8이면 최신 버젼으로 업데이트 해줍니다.
    1. 8 버젼이면 아래와 같은 에러가 발생합니다.
        
        ‘java.lang.UnsupportedClassVersionError: ch/qos/logback/classic/spi/LogbackServiceProvider has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0’
        
3. Source code에 example 폴더에 application.conf 파일을 kafka-lag-exporter 폴더로 이동해줍니다.
4. 아래 명령어를 실행해줍니다.
    
    ```bash
    $ ./bin/kafka-lag-exporter -Dconfig.file=/Users/jsh/kafka/kafka-lag-exporter/application.conf
    ```
