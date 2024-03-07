---
layout: post
title:  "Apache Kafka"
summary: "What is Apache Kafka?"
author: seunghwan
date: '2024-03-07 00:00:00 +0530'
category: ['Apache Spark']
tags: Pyspark
usemathjax: false
permalink: /blog/Apache-Kafka/
---

## Apache Kafka란?
Apache Kafka는 시작 또는 끝이 없는 스트리밍 이벤트 데이터나 일반 데이터를 수집, 처리, 저장하는데 사용되는 분산 메세지 스트리밍 플랫폼입니다. 대용량 실시간 로그처리에 특화된만큼 Fault-Tolerant 한 안정적인 아키텍처를 제공하며, 분산 애플리케이션 확장을 통해 스트리밍 이벤트를 분당 수십억 개까지 처리할 수 있고 필요한 모든곳에서 대규모 데이터를 동시에 이동할 수 있습니다. 이 외에도 스트리밍 데이터를 가져와 정확히 언제 무슨 일이 일어났는지 기록합니다. 이 기록은 변경 불가능한 커밋 로그라고 합니다. 즉 추가는 가능하지만 변경은 불가능합니다. 로그에 있는 데이터에 액세스할 수 있으며, 개수 제한 없이 여러 스트리밍 실시간 애플리케이션과 다른 시스템에서 로그에 데이터를 추가할 수도 있습니다.

* Producer/Consumber와 Publish/Subscribe을 같은 의미로 사용하고 있었습니다