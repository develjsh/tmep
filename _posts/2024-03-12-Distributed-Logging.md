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

## 설치 및 테스트

*현재 Docker Engine을 Mac silicon에서 사용할 수 있는 방법이 쉽지 않기 때문에 VM 생성이 가능한 환경이 갖추어진 후 설치 및 테스트를 진행해야 합니다.

1. memphis 사용 환경 구축
    1. 설치
        
        ```bash
        $ curl -s https://memphisdev.github.io/memphis-docker/docker-compose-latest.yml -o docker-compose-latest.yml
        
        # 해당 디렉토리에 docker-compose.yml 파일이 생겼는데, 아래 명령어를 실행 후 에러 나는 부분을 vi를 통해 bool 값을 string ""으로 바꿔줍니다.
        
        # 예시
        # ERROR: The Compose file './docker-compose.yml' is invalid because:
        # services.memphis-rest-gateway.environment.USER_PASS_BASED_AUTH contains true, which is an invalid type, it should be a string, number, or a null
        # services.memphis.environment.DOCKER_ENV contains true, which is an invalid type, it should be a string, number, or a null
        $ docker-compose -f docker-compose-latest.yml -p memphis up
        
        ```
        
    2. 확인
        1. http://localhost:9000/login 으로 접속합니다.
            
            *만약 VM을 사용헀으면 해당 VM IP 주소 입력 후 포트와 /login을 붙여주면 됩니다.
            
            1. 접속 아이디와 비밀번호는 [memphis.dev](http://memphis.dev) 접속 계정 정보인거 같지만, 새로 만드는 겁니다.
2. memphis client 생성
    1. http://localhost:9000/users 에서Producer와 Consumer client를 생성합니다.
        1. 비밀번호 생성은 아래 조건에 맞게 생성합니다.
            - Password must be at least 8 characters long, contain both uppercase and lowercase, and at least one number and one special character(!?-@#$%)
3. memphis schema 생성
    1. http://localhost:9000/schemaverse/list 에서 기본적으로 생성되어 있는 domo-schema를 아래와 같이 수정해줍니다.
        - Schema editor에서 코드 수정 후 아래에 Create version을 클릭해줘야 합니다.
        
        [기존]
        
        ```json
        {
        		"$id": "https://example.com/address.schema.json",
        		"description": "An address similar to http://microformats.org/wiki/h-card",
        		"type": "object",
        		"properties": {
        			"post-office-box": {
        			"type": "number"
        			},
        			"extended-address": {
        			"type": "string"
        			},
        			"street-address": {
        			"type": "string"
        			},
        			"locality": {
        			"type": "string"
        			},
        			"region": {
        			"type": "string"
        			},
        			"postal-code": {
        			"type": "string"
        			},
        			"country-name": {
        			"type": "string"
        			}
        		},
        		"required": [ "locality" ]
        	}
        ```
        
        [변경 후]
        
        ```json
        {
          "$id": "https://example.com/address.schema.json",
          "description": "An address similar to http://microformats.org/wiki/h-card",
          "type": "object",
          "properties": {
            "message": {
              "type": "string"
            }
          },
          "required": ["message"]
        }
        ```
        
4. Producer와 Consumer 코드 구현
    1. Go 설치
        
        * sudo apt-get install golang으로 설치하면 1.13 version으로 설치되는데 그러면 아래 코드에 없는 라이브러리가 있어 코드 실행이 힘들어집니다.
        
        *만약 먼저 설치했으면 아래 코드로 삭제해줍니다.
        
        ```bash
        # go언어 제거 하기
        $ sudo apt-get purge golang*
        $ sudo rm -rf /usr/local/go
        $ sudo rm -rf $(echo $GOPATH)
        
        # ~/bashrc 또는 ~/.profile에서 go 관련된 항목 제거(기존에 설정한 파일에서 작업)
        $ source ~/.profile
        $ source ~/.bashrc
        ```
        
        1. wget으로 직접 golang 파일을 받아와 설치할 수 있습니다.
            
            ```bash
            $ sudo apt-get update
            $ sudo apt-get -y upgrade
            
            $ wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
            $ sudo tar -xvf go1.21.6.linux-amd64.tar.gz -C /usr/local 
            $ echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile 
            $ source ~/.profile 
            $ go version
            ```
            
    2. Producer 코드를 작성합니다.
        
        ```go
        package main
        
        import (
        	"encoding/json"
        	"fmt"
        	"github.com/memphisdev/memphis.go"
        	"os"
        )
        
        type Message struct {
        	Message string `json:"message"`
        }
        
        func main() {
        	conn, err := memphis.Connect("localhost", "producer_1", memphis.Password(""), memphis.AccountId(1))
        	if err != nil {
        		os.Exit(1)
        	}
        	defer conn.Close()
        
        	p, err := conn.CreateProducer("default", "pro-1")
        	if err != nil {
        		fmt.Printf("Producer failed: %v", err)
        		os.Exit(1)
        	}
        
        	for i := 0; i < 1000000; i++ {
        		message := Message{
        			Message: fmt.Sprintf("msg - %d", i),
        		}
        
        		binary, err := json.Marshal(message)
        		if err != nil {
        			panic(err)
        		}
        
        		err = p.Produce(binary, memphis.AsyncProduce())
        		if err != nil {
        			panic(err)
        		}
        
        		fmt.Printf("[Produce] %s\n", message.Message)
        	}
        }
        ```
        
    3. Consumer 코드를 작성합니다.
        
        ```go
        package main
        
        import (
        	"context"
        	"fmt"
        	"os"
        	"time"
        
        	"github.com/memphisdev/memphis.go"
        )
        
        func main() {
        	conn, err := memphis.Connect("localhost", "consumer_1", memphis.Password(""), memphis.AccountId(1))
        	if err != nil {
        		os.Exit(1)
        	}
        	defer conn.Close()
        
        	consumer, err := conn.CreateConsumer("default", "con-1", memphis.ConsumerGroup("con"), memphis.PullInterval(1*time.Nanosecond), memphis.BatchSize(100))
        	if err != nil {
        		fmt.Printf("Consumer creation failed: %v", err)
        		os.Exit(1)
        	}
        
        	handler := func(msgs []*memphis.Msg, err error, ctx context.Context) {
        		if err != nil {
        			panic(err)
        		}
        
        		for _, msg := range msgs {
        			fmt.Printf("[Consume] %s\n", string(msg.Data()))
        			msg.Ack()
        		}
        	}
        
        	ctx := context.Background()
        	consumer.SetContext(ctx)
        	consumer.Consume(handler)
        	time.Sleep(30 * time.Minute)
        }
        ```
        
5. 동장 테스트 및 모니터링
    1. Producer와 Consumer로 작성한 코드를 실행해줍니다.
        
        ```bash
        # 실행 방법 예시입니다.
        $ go mod init m_producer
        $ go mod tidy
        $ go run [go 파일 이름]
        ```
        
        [error]
        
        - m_producer.go:6:2: cannot find package "[github.com/memphisdev/memphis.go](http://github.com/memphisdev/memphis.go)" in any of:
            - go install [github.com/memphisdev/memphis.go@lates](http://github.com/memphisdev/memphis.go@lates)t 명령어를 통해 해당 ~/producer/ 디렉토리에 설치해줍니다.
        - 
    2. 코드 실행 완료 후 http://localhost:9000/stations/default 에 접속 한 후 코드 실행 결과를 모니터링 할 수 있습니다.