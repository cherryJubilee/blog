---
draft: false
authors:
    - hyewon
date: 2024-02-23
categories:
    - devops
---

# AWS EC2에 Docker를 활용해 배포하기

> EC2, AWS EC2에 Docker를 활용해 배포하기

<!-- more -->

> 이름 및 태그

-   ec2마다 명칭을 부여하는 것
-   wedder-server

> OS선택

-   ubuntu(윈도우나 맥에 비해 가볍운, 설정이 많이 안되어 있어서)

> 인스턴스 유형

-   인스턴스 : ec2에서 빌리는 컴퓨터 1대
-   유형 : 컴퓨터 사양
    -   t2.micro

> 키페어

-   ec2 컴퓨터에 접근하기 위한 비밀번호 (key)
-   어떤 ec2에 접근하기 위한 key였는지 식별 가능한 키페어이름 작성
-   wedder-server-key

> 네트워크 설정

-   편집 클릭
-   VPC
-   보안그룹

    -   보안그룹 이름 : `wedder-server-security-group`
    -   IP, port를 설정한다.
    -   인바운드 트래픽 (외부에서 ec2 인스턴스로 보내는 트래픽)
    -   아웃바운드 트래픽 (ec2 인스턴스에서 외부로 보내는 트래픽)

-   인바운드 예시

    > security group1

    -   ssh(개발자가 ec2에 원격 접속을 위한), 22번 포트
    -   위치무관

    > security group2

    -   HTTP, 80번 포트
        -   백엔드 서버를 80번 포트에 띄울 예정이기 떄문
    -   위치무관

> 스토리지 구성

-   `EBS(Elastic Block Storage)` : EC2 안에 부착되어 있는 일종의 하드디스크
-   크기 : 30GIB
-   볼륨유형 : gp3

퍼블릭 IPv4 주소 : 생성한 ec2의 ip 주소
인스턴스 종료 = ec2 삭제

🎯 탄력적 IP가 필요한 이유?
EC2 인스턴스를 생성하면 퍼블릭 IP를 할당받는다. 하지만 이렇게 할당받은 IP는 임시적인 IP이다. EC2 인스턴스를 잠깐 중지시켰다가 다시 실행시켜보면 IP가 바뀌어있다. EC2 인스턴스를 중지시켰다가 다시 실행시킬 때마다 IP가 바뀌면 굉장히 불편하다. 그래서 중지시켰다가 다시 실행시켜도 바뀌지 않는 고정 IP를 할당받아야 한다.

탄력적 IP 할당

-   이름 편집 : wedder-server-ip
-   작업 클릭
-   탄력적 IP주소 연결
-   인스턴스 선택 : 아까 만든 ec2선택
-   해당 ec2의 퍼블릭 IPv4 주소가 탄력적 IP와 똑같이 바뀐다.
-   이제 IP가 고정이 된 것이다.

## 📌 AWS EC2에 서버 배포하기

## 📌 AWS EC2에 Docker를 활용해 배포하기

1. ec2 생성 후 접속
2. ubuntu에서 Docker, Docker Compose 설치하기

<details>
<summary>Ubuntu에서 Docker, Docker Compose 설치하기</summary>
```shell
$ sudo apt-get update && \
	sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common && \
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
	sudo apt-key fingerprint 0EBFCD88 && \
	sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
	sudo apt-get update && \
	sudo apt-get install -y docker-ce && \
	sudo usermod -aG docker ubuntu && \
	newgrp docker && \
	sudo curl -L "https://github.com/docker/compose/releases/download/2.27.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
	sudo chmod +x /usr/local/bin/docker-compose && \
	sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

잘 설치 되었는지 확인하기

```shell
$ docker -v # Docker 버전 확인
$ docker compose version # Docker Compose 버전 확인
```

</details>

### ECR(Elastic Container Registry)

-   Dockerhub와 같은 역할
-   Dockerhub대신 ECR을 사용하는 이유? - 최근에는 AWS 클라우드 환경에서 인프라를 구축하는 일이 많아졌다. AWS ECR을 사용하면 다른 AWS Resource와의 연동이 편하고, AWS 내에서 한 번에 관리할 수 있기에 편하다는 장점이 있다.

    ![alt text](img/image36.png)

-   Docker의 가장 큰 장점은 **이식성**이다. **Docker만 깔려있으면 어디에서든 내가 원하는 프로젝트를 실행시킬 수 있다는 게 장점**이다. 이 때 Github을 활용해 프로젝트 코드 전체를 EC2로 옮겨 Docker 기반으로 실행시켜도 된다. 하지만 프로젝트에서 필요한 코드에 대해서만 Docker 이미지로 빌드해, EC2에서는 그 이미지만 다운받아서 실행시키는 게 훨씬 심플하다.

-   정리하자면 **AWS ECR을 배우는 이유는 훨씬 간단하게 프로젝트를 배포하고 실행시키기 위해서이다.**

3. AWS CLI 설치

-   https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html
-   맥

    ```bash
    $ brew install awscli
    $ aws --version # 잘 출력된다면 정상 설치된 상태
    ```

4. IAM 생성하기

-   ECR에 접근하기 위한 권한을 얻기 위해 만든다.
-   사용자 -> 사용자 생성 -> 사용자 이름 : wedder-hyewon
-   직접 정책 연결 -> AmazonEC2ContainerRegistryFullAccess 선택
-   사용자 생성 완료
-   보안자격 증명 -> 액세스 키
