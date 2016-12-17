---
layout: post
title: "Docker Container"
tags: [docker]
description: >

---

## Docker Container Lifecycle

이미지가 생성되면 이미지를 바탕으로 컨테이너를 생성할 수 있다. 컨테이너의 Lifecycle은 아래와 같다.

![Docker Container Lifecycle][docker-container-lifecycle]

### 컨테이너 생성(docker create)

Docker 이미지로 컨테이너를 생성한다. `docker create` 명령어를 실행하면 이미지에 포함된 linux 디렉토리 및 파일 집합의 스냅샷을 만든다.

### 컨테이너 생성 및 구동(docker run)

Docker 이미지에서 컨테이너를 생성하여 컨테이너상에서 프로로세스를 구동한다.

#### 생성 및 구동

|옵션|설명|
---|---
-a, --attach=[STDIN or STDOUT or STDERR]|표준입력(STDIN), 표준출력(STDOUT), 표준 에러 출력(STDERR)을 연결
--cidfile="file name"|컨테이너 ID를 파일로 출력
-d, --detach=false|컨테이너를 생성하여 백그라운드에서 실행
-i, --interactive=false|컨테이너 표준 입력 열기
-t, --tty=false|tty(단말 디바이스)를 사용
--name|컨테이너 명

```bash
docker run -it --name "testCal" centos /bin/cal
```

![test calendar][test-cal]

```bash
docker run -it --name "testBash" centos /bin/bash
```

![test bash][test-bash]

#### 백그라운드 실행

```bash
docker run [option] <image>[:tag] [cmd]
```

|옵션|설명|
---|---
-d,--detach|백그라운드에서 실행
-u,--user="사용자명"|사용자명을 입력
--restart=[no or on-failure or on-failure:횟수n or always]|커맨드 실행 결과에 따라 재기동
--rm|커맨드 실행 완료 후 컨테이너 자동 삭제

```bash
docker run -d centos /bin/ping localhost
```

![docker run background][docker-run-background]

실행이 완료된 후 컨테이너를 자동으로 삭제하고자 할 때에는 `rm`옵션을 사용한다. 또한, 커맨드 실행 결과에 따라 컨테이너를 재구동시키고자 할때에는 `restart` 옵션을 사용한다.

`restart`옵션은 아래와 같다.

|값|설명|
---|---
no|재구동하지 않음
on-failure|종료 status가 0이 아닌 경우 재구동
on-failure:횟수n|종료 status가 0이 아닌 경우 n번 재구동
always|항상 재구동

#### 컨테이너 네트워크 설정

```bash
docker run [option] <image>[:tag] [cmd]
```

|옵션|설명|
---|---
--add-host=[Host Name:IP Address]|컨테이너의 /etc/hosts에 Host Name과 IP Address를 설정
--dns=[IP Address]|DNS 서버의 IP Address를 설정
--expose=[Port Number]|Port Number 할당
--mac-address=[MAC Address]|Container의 MAC Address 설정
--net=[bridge or none or container:<name or d> or host]|컨테이너 네트워크 설정
-h,--hostname="Host Name"|컨테이너의 Host Name 설정
-P,--publish-all=[true or false]|임의의 포트를 컨테이너에 할당
-p [Host Port Number]:[Container Port Number]|Host와 Container의 Port를 매핑
--link=[컨테이너명:alias]|다른 컨테이너에서 액세스 시 이름 설정

__Port Mapping__

```bash
docker run -d -p 9000:80 httpd
```

호스트 포트 `9000`과 컨테이너 포트`80`에 매핑한다. 호스트 `9000`으로 액세스 하면 컨테이너 상의 `80`포트로 연결된다. 임의의 포트를 할당할 때에는 `P` 옵션을 사용한다.

__DNS Server__

```bash
docker run --dns=192.168.1.1 httpd
```

__MAC Address__

```bash
docker run -it --mac-address="92:d0:c6:0a:29:33" centos
```

__Host Name__

```bash
docker run -it --add-host=test.com:192.168.1.1 centos
```

__net option__

|값|설명|
---|---
bridge|bridge 접속(default) 사용
none|네트워크에 접속하지 않음
container:[name or id]|다른 컨테이너의 네트워크를 사용
host|컨테이너가 호스트 OS의 네트워크를 사용

#### 리소스 설정

```bash
docker run [option] <image>[:tag] [cmd]
```

__option__

|option|description|
---|---
-c,--cpu-shares=0|CPU resource 분배
-m,--memory=[usage]|메모리 사용량 제한(단위는 b, k, m, g)
-v,--volume=[host directory]:[container directory]|호스트와 컨테이너의 디렉토리 공유

__cpu & memory__

```bash
docker run --cpu-shares=512 --memory=512m centos
```

Docker 리소스를 제한하는 기능은 `linux`의 `cgroups` 기능을 사용한다.

`Host OS`와 `Container`내의 디렉토리를 공유하고자 할 때는 volume option을 사용한다.

__volume__

```bash
docker run -v /c/Users/user/workspace:/var/www/html httpd
```

#### 환경 설정

```bash
docker run [option] <image>[:tag] [cmd]
```

|option|description|
---|---
-e, --env=[환경변수]|환경변수 설정
--env-file=[파일명]|파일에서 환경변수 설정
--privileged=[true or false]|privileged 모드에서 구동(호스트의 커널 기능도 사용 가능)
--read-only=[true or false]|컨테이너의 파일 시스템을 read-only로 설정
-w, --workdir=[경로]|컨테이너의 작업 디렉토리를 설정

```bash
docker run -it -e foo=bar centos /bin/bash
```

```bash
docker run -it --env-file=env_list centos /bin/bash
```
---

### 컨테이너 목록 확인(docker ps)

```bash
docker ps [option]
```

__option__

|option|description|
---|---
-a, --all=false|구동, 중지 상태의 모든 컨테이너를 표시
--before=""|입력한 컨테이너명 또는 ID보다 이전에 구동된 컨테이너를 표시
--since=""|입력한 컨테이너명 또는 ID보다 이후에 구동된 컨테이너를 표시
-l, --latest=false|마지막에 구동된 컨테이너를 표시
-f, --filter '[key]=[value]'|목록에 표시할 컨테이너를 필터링
--format '[key]=[value]'|폭록에 표시할 포맷을 설정
--no-trunc=false|생략된 정보 없이 모두 표시
-q, --quiet=false|컨테이너 ID만 표시
-s, --size=false|파일 사이즈를 표시


__result(option key)__

|item|description|
---|---
CONTAINER ID| 컨테이너 ID
IMAGE|컨테이너 기반이 된 이미지
COMMAND|컨테이너에서 실행중인 커맨드
CREATED|컨테이너 생성 후 경과 시간
STATUS|컨테이너 상태(restarting or running or paused or exited)
PORTS|할당된 포트
NAMES|컨테이너 명


__format__

|placeholder|description|
---|---
.ID|컨테이너 ID
.Image|컨테이너의 기반이 된 이미지
.Command|실행 커맨드
.CreatedAt|컨테이너가 생성된 시간
.RunningFor|컨테이너 구동 시간
.Ports|할당된 포트
.Status|컨테이너 상태
.Size|컨테이너 디스크 사이즈
.Labels|컨테이너의 모든 라벨
.Label|컨테이너 라벨

```bash
docker ps -a --format "table {{.ID}}\t{{.Status}}"
```
---

### 컨테이너 구동 확인(docker stats)

```bash
docker stats <컨테이너명 또는 ID>
```

__result__

|item|description|
---|---
CONTAINER|컨테이너명 또는 ID
CPU %|CPU 사용률
MEM USAGE/LIMIT|메모리 사용량/컨테이너에서 사용할 수 있는 메모리 제한
MEM %|메모리 사용률
NET I/O|네트워크 I/O

---

### 컨테이너 구동(docker start)

중지중인 컨테이너를 구동한다. 컨테이너명 또는 ID를 입력하여 구동한다.

```bash
docker start [option] <컨테이너명 또는 ID>
```

__option__

|option|description|
---|---
-a, --attach=false|표준 출력, 표준 에러를 연결
-i, --interactive=false|컨테이너 표준 입력을 연결

---

### 컨테이너 중지(docker stop)

구동중인 컨테이너를 중지한다. 컨테이너명 또는 ID를 입력하여 중지한다.

```bash
docker stop [option] <컨테이너명 또는 ID>
```

__option__

|option|description|
---|---
-t, --time=10|컨테이너 중지 시간을 지정(default: 10)

---

### 컨테이너 재시작(docker restart)

```bash
docker restart [option] <컨테이너명 또는 ID>
```

__option__

|option|description|
---|---
-t, --time=10|컨테이너 재시작 시간을 지정(default: 10)

---

### 컨테이너 삭제(docker rm)

컨테이너를 삭제한다. 삭제하는 컨테이너는 사용 중지중이어야 한다.

```bash
docker rm [option] <컨테이너명 또는 ID>
```

__option__

|option|description|
---|---
-f, --force=false|구동 중인 컨테이너를 강제 삭제
-v --volume=false|할당된 볼륨을 삭제

---

### 컨테이너 일시정지 및 재시작(docker pause/docker unpause)

```bash
docker pause <컨테이너명 또는 ID>
docker unpause <컨테이너명 또는 ID>
```

---

## Docker Container Usage

### 컨테이너 접속(docker attach)

```bash
docker attach <컨테이너명 또는 ID>
```

`Ctrl` + `P`, `Ctrl` + `Q`로 종료

---

### 컨테이너의 프로세스 실행(docker exec)

```bash
docker exec [option] <컨테이너명 또는 ID> <cmd> [value]
```

__option__

|option|description|
---|---
-d, --detach=false|커맨드를 백그라운드에서 실행
-i, --interactive=false|컨테이너 표준 입력 열기
-t, --tty=false|tty(단말디바이스) 사용

---

### 컨테이너의 프로세스 확인(docker top)

```bash
docker top <컨테이너명 또는 ID>
```

---

### 컨테이너의 포트 상태 확인(docker port)

```bash
docker port <컨테이너명 또는 ID>
```

---

### 컨테이너명 변경(docker rename)

```bash
docker rename <old> <new>
```

---

### 컨테이너 내에서 파일 복사

```bash
docker cp <컨테이너명 또는 ID>:<컨테이너 내의 파일 경로> <호스트 디렉토리 경로>
docker cp <호스트 파일> <컨테이너명 또는 ID>:<컨테이너 내의 파일 경로>
```

---

### 컨테이너 내에서 파일 변경 이력 확인(docker diff)

```bash
docker diff <컨테이너명 또는 ID>
```

__구분__

|구분|description|
---|---
A|파일 추가
D|파일 삭제
C|파일 변경

---

### docker version

Docker의 버전 확인

```bash
docker version
```
---

### Docker 실행 환경 확인(docker info)

```bash
$ docker info
Containers: 6
 Running: 0
 Paused: 0
 Stopped: 6
Images: 7
Server Version: 1.12.3
Storage Driver: aufs
 Root Dir: /var/lib/docker/aufs
 Backing Filesystem: extfs
 Dirs: 71
 Dirperm1 Supported: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: null bridge host overlay
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Security Options: seccomp
Kernel Version: 4.4.27-moby
Operating System: Alpine Linux v3.4
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 1.951 GiB
Name: moby
ID: LRQA:PHWE:COK5:5FCF:7KYP:ZWQ2:346W:PP5A:GMHM:N3WP:AKV4:YYRZ
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): true
 File Descriptors: 16
 Goroutines: 29
 System Time: 2016-12-17T05:47:28.308350112Z
 EventsListeners: 1
No Proxy: *.local, 169.254/16
Registry: https://index.docker.io/v1/
WARNING: No kernel memory limit support
Insecure Registries:
 127.0.0.0/8
```

---

## 컨테이너에서 이미지 생성

### 컨테이너에서 이미지 생성(docker commit)

```bash
docker commit [option] <컨테이너명 또는 ID> [image][:tag]
```

__option__

|option|description|
---|---
-a, --author=""|생성자
-m, --message=""|메시지
-p, --pause=true|컨테이너를 일시 중지한 후 commit

---

### 컨테이너를 tar 파일로 저장(docker export)

```bash
docker export <컨테이너명 또는 ID>
```

---

### tar 파일에서 이미지 생성(docker import)

```bash
docker import <파일 또는 URL> - [image][:tag]
```

### 이미지 저장(docker save)

```bash
docker save [option] <file> [image]
```

### 이미지로 되돌리기(docker load)

```bash
docker load [option]
```

---

[docker-container-lifecycle]: /public/img/docker-container/docker-container-lifecycle.png
[test-cal]: /public/img/docker-container/test-cal.png
[test-bash]: /public/img/docker-container/test-bash.png
[docker-run-background]: /public/img/docker-container/docker-run-background.png
