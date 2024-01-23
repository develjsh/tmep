---
layout: post
title:  "Setting up Big Data Analysis Environment"
summary: "Learn how to set up big data analysis environment"
author: seunghwan
date: '2024-01-22 00:00:00 +0530'
category: ['big_data', 'hadoop', 'spark']
tags: big_data
#thumbnail: /assets/img/posts/code.jpg
usemathjax: false
permalink: /blog/adding-categories-tags-in-posts/
---
# Setting up Big Data Analysis Environment

## 0강. 고가용성 Hadoop 클러스터 환경 구축합니다.
- 총 5개의(nn1, nn2, dn1, dn2, dn3) vm 이 필요합니다.

## 1강. GCP VM 인스턴스 배포
1. Compute Engine 을 클릭 후 인스턴스 만들기를 클릭합니다.
2. 각 원하는 구성을 선택해줍니다. 아래 내용은 참고 내용 입니다.
    - 리전: 서울
    - 머신 구성: E2
    - 머신 유형: e2-standard-4(vCPU 4개, 코어 2개, 메모리 16GB)
    - 엑섹스 범위: 기본 엑세스 허용
    - 방화벽: HTTP 트래픽 허용, HTTPS 트래픽 허용
3. ssh-keygen 을 통해 ssh key 를 생성해 줍니다.
    - $ ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]
        - [타입명] : key 생성 type을 선택하는 옵션입니다.
        - f [파일명] : 생성할 key 파일의 이름을 설정하는 옵션입니다.
        - C [내용] : 사용자에 대한 주석을 입력합니다.
        - b [숫자] : type의 bytes를 설정합니다. rsa 암호화 방식은 기본 값이 2048이며, 4096으로 더 안전한 키를 생성할 수도 있습니다.
    - ssh key 가 public(.pub) 과 private 이 생성됩니다.
4. 생성된 ssh key 를 GCP 콘솔 서버에 등록합니다.
    - Compute Engine 에서 ‘VM 인스턴스’ 있는 사이드 바에 메타데이터가 존재합니다. 메타데이터에 들어간 후, SSH 키 태그를 누릅니다.
    - 추가를 합니다.
5. ~/.ssh 폴더에 config 파일을 만들어 ssh 연결 설정을 해줍니다.
    ```yml
    ---
    Host nn1
        HostName 10.10.171.101
        User jay
        IdentityFile ~/.ssh/rsa_big_data
    ---
    ```
    **Troubleshooting
    VM 인스턴스를 정지 후 새로 시작하면 외부 IP 주소가 바뀌기 때문에 재실행 시 config 설정 값도 수정해줘야 합니다.
6. ssh nn1 을 터미널에 입력 후 실행해 Server 에 연결 되는지 확인합니다.