---

title: "하이퍼레저 패브릭 환경 구성"
comments: true
categories:
  - hyperledger
tags:
  - blockchain
  - hyperledger
  - hyperledger-fabric
  - fabric
date: "2020-05-15 14:00"
---

하이퍼레저 패브릭 환경 구성

※ 환경 구성에 대한 정보는  [하이퍼레저 패브릭 공식 docs](https://hyperledger-fabric.readthedocs.io/en/release-2.0/prereqs.html) 에서 발췌 하였으며 이번에 진행하는 버전은 v2.0.0 입니다.

#### [구성환경]

1. Oracle VM
2. Ubuntu 18.04

#### [필수 구성 요소]

1. git
2. curl
3. docker
4. Go language
5. docker-compose
6. node, npm

※ root 계정으로 진행하지 않는것을 추천 

## git  설치

※ 설치관련 자세한 정보는 [Git홈페이지](https://git-scm.com/downloads) 참조

```shell
sudo apt-get install git
```

## curl 설치

```shell
sudo apt-get install curl
```

## docker  설치

##### 도커 repository 설정

1. apt 패키지 업데이트 & HTTPS repository 사용을 위한 패키지 설치

   ```shell
   $ sudo apt-get update
   $ sudo apt-get install \
   	apt-transport-https \
   	ca-certificates \
   	curl \
   	gnupg-agent \
   	software-properties-common
   ```

2. 도커 공식 GPG Key 추가

   ```shell
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

3. stable repository 설정

   ```shell
   $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

##### 도커 설치

```shell
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

##### 현재 계정에 도커 권한 주기 

```shell
sudo usermod -aG docker $USER
```

##### docker-compose 설치

```shell
$ sudo apt-get install docker-compose
```

##### Python 설치 유무 확인(파이썬 버전 확인)

```shell
$ python -V
```

※ docker-compose를 설치할때 python2.7 버전이 설치됨 docker-compose 설치 이후 파이썬 버전 확인시 확인이 안될경우 파이썬 설치

```shell
$ sudo apt-get install python
```

**<u>※ 도커 설치 후 우분투 계정 재 로그인 해야 계정에 도커 권한이 적용됩니다.</u>**

※ 설치에 대한 자세한 사항은 [도커홈페이지](https://www.docker.com/get-started) 참조

## Go Language 설치

※ 설치관련 자세한 정보는 [Go 홈페이지](https://golang.org/dl/) 참조

##### Go 설치 파일 다운로드

```shell
$ wget https://dl.google.com/go/go1.14.3.linux-amd64.tar.gz
```

##### 설치 파일 압축 해제

```shell
$ sudo tar -C /usr/local -xzf go1.14.3.linux-amd64.tar.gz
```

##### Go  Workspace 용 폴더 설정(샘플소스 저장)

```
$ mkdir go			//Optional
```

##### Go 환경변수 설정

```shell
$ echo "export GOPATH=$PWD/go" >> ~/.profile				    // Go Workspace 설정
$ echo "export GOROOT=/usr/local/go" >> ~/.profile				// Go binary 경로 설정
$ echo "export PATH=$PATH:$GOROOT/bin" >> ~/.profile	    	 // Go 명령어 사용을 위해 PATH 설정
$ source ~/.profile											  // 환경변수 설정 저장
```

## Node.js 설치

※ 설치관련 자세한 정보는 [Node 홈페이지](https://nodejs.org/en/download/) 참조(운영체제에 맞게 설치)

※ 데비안,우분투 Node 설치관련 자세한 정보는 [Node  Github페이지](https://github.com/nodesource/distributions/blob/master/README.md) 참조

```shell
sudo apt-get install nodejs
```

node 설치시 npm은 같이 설치됨 

지금까지 설치한 환경 확인 

```shell
git --version
curl --version
docker --version
docker-compose --version
go version
node --version
npm -V
```

버전확인이 안될 시 우분투 재시작 또는 로그아웃 후 재로그인 후 다시 확인 

위 프로그램들이 정상적으로 설치 되었을 경우 

## Hyperledger Fabric Samples, Binaries, Docker Image 다운로드 및 설치 

##### fabric 폴더 생성 (계정의 home 폴더 에서 진행)

```shell
cd
mkdir fabric
```

##### fabric 설치

- 위에서 계정에 docker 관련 권한을 주지 않았을 경우 다운로드 후 bash 실행 과정에서 permission 오류가 날 수 있으니 꼭 권한을 주고 진행 할 것 
- 해당 다운로드 진행 시 sample, binaries, docker image 가 동시에 받아진다

생성한 fabric 폴더 내에서 진행한다

```shell
cd fabric
```

<최신버전>

```shell
curl -sSL https://bit.ly/2ysbOFE | bash -s
```

<특정 버전 설치 시>

```shell
# curl -sSL https://bit.ly/2ysbOFE | bash -s -- <fabric버전> <fabric-ca버전> <thirdpatry 버전>
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.0.0 1.4.4 0.4.18
```

다운로드 및 설치가 완료 되면 fabric-samples 폴더가 생성되고 그 안에 아래 그림과 같이 생성됨을 확인 한 뒤

![](D:\Node.js\workspace\simplehanlab.github.io\assets\images\hyperledger-fabric\chapter1\캡처.PNG)

 /bin 폴더 내에 해당 파일이 존재하는지 확인 

- `configtxgen`,
- `configtxlator`,
- `cryptogen`,
- `discover`,
- `idemixgen`
- `orderer`,
- `peer`,
- `fabric-ca-client`

![](D:\Node.js\workspace\simplehanlab.github.io\assets\images\hyperledger-fabric\chapter1\2.PNG)

해당 바이너리를 사용하지 위해 환경 변수 설정

```shell
echo "export PATH=<path to fabric download location>/bin:$PATH" >> ~/.profile
```

위 과정이 에러 없이 모두 성공적으로 진행 되었다면 하이퍼 레저 패브릭을 사용하기 위한 환경은 구성 된 것입니다. 다음 포스트에서는 하이퍼레저 패브릭에서 제공하는 test-network 를 통해 네트워크를 실행 테스트를 진행해 보겠습니다. 

다음 포스트를 기다리기 힘드신 분들은  [하이퍼레저 패브릭 공식 docs](https://hyperledger-fabric.readthedocs.io/en/release-2.0/test_network.html) 를 보고 진행해 보세요.