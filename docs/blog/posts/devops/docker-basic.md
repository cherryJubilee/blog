---
draft: false
authors:
    - hyewon
date: 2024-07-30
categories:
    - devops
---

# Docker 기본

> 비전공자도 이해할 수 있는 Docker 입문/실전

<!-- more -->

## ✅ Docker CLI 익히기

중단된 컨테이너 한번에 삭제

```bash
docker rm  ${docker ps -qa}
```

실행되고 있는 컨테이너 중단 + 제거 한번에 하기
도커 컨테이너는 실행되고 있는 상태에서는 지울 수 없다. (꼭 중단 시키고 제거해야됨)

```bash
docker rm -f  도커컨테이너명
```

docker run

1. 도커 이미지 다운로드(pull) 받아서 컨테이너 생성(create)
2. create만 해도 이미지가 없다면 알아서 docker hub로 부터 이미지를 pull받은 후 컨테이너를 생성한다.
3. 컨테이너 실행(start)

포그라운드
백그라운드

## ✅ 컨테이너에서 발생하는 로그 확인

로그 확인

```bash
docker logs 69d
```

로그 마지막 10줄 확인

```bash
docker logs --tail 10 69d
```

실시간으로 생성되는 로그 확인

```bash
docker logs --tail 0 -f 컨테이너아이디
```

```bash

```

## ✅ 실행중인 컨테이너 내부 환경에 접속하는 방법

![alt text](img/image3.png)

```bash
docker exec -it 컨테이너아이디 bash
```

다시 내 PC 환경(host os)으로 나오기
![alt text](img/image4.png)

```bash
exit
```

### 예제 redis 실행해보기

![alt text](img/image5.png)

1. docker hub에서 redis 이미지 검색해보기

    https://hub.docker.com/search?q=redis

2. 도커 이미지 다운 + 컨테이너 생성 + 컨테이너 실행

    - 이미지 확인
    - 컨테이너 확인
    - 로그 확인

    ```bash
    docker run -d -p 6379:6379 redis
    docker image ls
    docker ps
    docker logs 4f3
    ```

3. 레디스 컨테이너 접근

    ```bash
    docker exec -it 4f3 bash
    ```

4. 레디스에 직접 접근

    ```bash
    root@4f3f8cc95a6f:/# redis-cli
    ```

5. 이미지 삭제, 컨테이너 삭제

    ```bash
    docker rm -f 4f3
    docker ps
    docker image ls
    docker image rm redis
    docker rm -f 8d3
    ```

## Docker Volume(도커 볼륨)

-   도커 이미지를 변경하고싶을 때 이미지를 수정하는 것 보다 새로 만드는 것이 효율적이라고 생각.
