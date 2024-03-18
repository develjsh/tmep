---
layout: post
title:  "Apache Avro"
summary: "Apache Avro?"
author: seunghwan
date: '2024-03-18 00:00:00 +0530'
category: ['Apache Avro']
tags: Apache Avro
usemathjax: false
permalink: /blog/Apache-Avro/
---

## 정의

데이터 직렬화 시스템으로서, 대량의 데이터를 효율적으로 저장하고 전송하기 위한 개방형 데이터 직렬화 프레임워크입니다.

## Avro의 Record 자료형

Avro에는 null, boolean, int, long, float, double, bytes, string 의 원시 타입과 record, enums, arrays, maps, unions 등의 복합 타입이 존재합니다.

record 타입은 아래와 같은 구성 요소를 가지고 있습니다.

```json
{
    "namespace": "example.avro",
    "type": "record",
    "name": "User",
    "fields": [
        {"name": "name", "type": "string"},
        {"name": "favorite_number",  "type": ["int", "null"]},
        {"name": "favorite_color", "type": ["string", "null"]}
    ]
}
```

- name — 문자열 : 스키마 이름 (필수)
- namespace — 문자열 : 패키지 (선택)
- doc — 문자열 : 스키마에 대한 설명 (선택)
- aliases — 배열 : 이 레코드에 대한 대체 이름 (선택)이름을 바꾼 경우 여기에 구버전 이름을 기입하여 사용할 수 있다.
- fields — 배열 : 필드에 대한 리스트 (필수)
    - fields 하위 요소
        - name — 문자열 : 필드의 이름 (필수)
        - doc — 문자열 : 필드에 대한 설명 (선택)
        - type — 스키마 : 필드의 타입, 타입 혹은 스키마를 여러개 기입할 수 있다 (필수)
        - default : 필드의 기본 값 (선택)
        - order : 데이터를 정렬할 방향을 정한다.ascending이 기본이고, descending, ignore 옵션이 있다. 직렬화 될 때 정렬이 수행된다. (선택)