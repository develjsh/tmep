---
layout: post
title:  "Distributed Logging"
summary: "Distributed Logging"
author: seunghwan
date: '2024-03-12 00:00:00 +0530'
category: ['Distributed Logging']
tags: Distributed Logging
usemathjax: false
permalink: /blog/Distributed-Logging/
---

## 분산 로깅이 필요한 이유

분산되어 있는 시스템 전체의 상태를 이해하고 문제의 원인을   빠르게 찾기 위해서입니다.

## 분산 로깅이 개발

### 사용되는 도구
- Go
- memphis
    - CNCF에 의해 관리되는 NATS 프로젝트를 기반으로 Go로 작성된 프로젝트입니다.
    - 분산 로깅에 필요한 내구성이나 장애 복구가 적합하지 않은 NATS를 분산 로깅에 적합하게 해줍니다.

