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

## 라이선스
- memphis: Boost Software License 1.0 (BSL1.0)

*Boost Software License 1.0 (BSL1.0): 

| 복제, 배포, 수정의 권한 허용 | 가능 |
| --- | --- |
| 배포시 라이선스 사본 첨부 | 가능 |
| 저작권 고지사항 또는 Attribution 고지사항 유지 | 가능 |
| 배포시 소스코드 제공의무(Reciprocity)와 범위 | 명시되어있지않음 |
| 조합저작물(Lager Work) 작성 및 타 라이선스 배포 허용 | 조건부 가능 |
| 수정 시 수정내용 고지 | 명시되어있지않음 |
| 명시적 특허 라이선스의 허용 | 명시되어있지않음 |
| 라이선시가 특허소송 제기 시 라이선스 종료 | 명시되어있지않음 |
| 이름, 상표, 상호에 대한 사용제한 | 명시되어있지않음 |
| 보증의 부인 | 가능 |
| 책임의 제한 | 가능 |

참조: https://www.olis.or.kr/license/Detailselect.do?lId=1070&mapCode=010068,010107