# AI/SW 워크스테이션 구축

## 1. 프로젝트 개요
Docker를 활용하여 컨테이너 기반 개발 워크스테이션을 구축하고, 이미지 생성, 컨테이너 실행, 포트 매핑, 볼륨을 통해 동일한 실행 환경을 재현하는 방법을 학습한다.

## 2. 실행 환경
- OS : macOS
- 쉘 : zsh
- Docker 버전 : 28.5.2
- Git 버전 : 2.53.0

## 3. 수행 체크리스트
### - ✅ Git 설정 및 GitHub 연동

Git 설정, GitHub 연동 방법 (history로 대체)

```bash 
    1  cd ~
    2  ls
    3  mkdir docker-mission
    4  cd docker-mission
    5  pwd
    6  git init
    7  git config --global init.defaultBranch main
    8  touch README.md
    9  ls
   10  git add .
   11  ls
   12  git commit -m "init project"
   13  git config --global user.name ...
   14  git config --global user.email ...@....com
   15  git commit --amend --reset-author
   16  git config --global user.name
   17  git config --global user.email
   18  git remote add origin https://github.com/.../....git
   19  git branch
   20  git push -u origin master
```

<br>

디렉토리 구조 및 구성 기준

```
.                        # docker-mission
├── Dockerfile           # 컨테이너 빌드를 위한 설정 파일
└── test/                # 테스트 관련 소스 코드 폴더
    └── index.html       # 웹 서버 메인 페이지

홈 디렉토리에 docker-mission 폴더 생성 후 GitHub와 연동 
바인드 마운드 테스트, 볼륨 영속성 테스트 등 여러 테스트를 위해 test 폴더를 별도로 구성 
```


<br>

확인 

```bash
username@c4r2s8 docker-mission % git config --list
credential.helper=osxkeychain
init.defaultbranch=main
user.name=...
user.email=...
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/....git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
user.name=...
user.email=...
branch.master.remote=origin
branch.master.merge=refs/heads/master
```

<details>
    <summary>연동 이미지</summary> 
<p>
<img width="825" height="484" alt="Screenshot 2026-04-02 at 11 39 36 AM" src="https://github.com/user-attachments/assets/8e02dffd-2d87-487c-af35-d516a1ae131d" />
</p>
<p>
<img width="425" height="240" alt="Screenshot 2026-04-02 at 11 21 16 AM" src="https://github.com/user-attachments/assets/2beb6cc1-9120-400e-9e30-5d432b9bbba7" />
</p>
</details>

<br>

### - ✅ 터미널 기본 조작 및 폴더 구성
```bash
username@c4r2s8 docker-mission % pwd                 # 현재 위치 확인
/Users/username/docker-mission
username@c4r2s8 docker-mission % ls -la              # 파일 목록 확인 (숨긴 파일 포함)
total 0
drwxr-xr-x   4 username  groupname  128 Apr  1 09:34 .
drwxr-x---+ 22 username  groupname  704 Apr  1 09:41 ..
drwxr-xr-x  12 username  groupname  384 Apr  1 09:53 .git
-rw-r--r--   1 username  groupname    0 Apr  1 09:34 README.md
username@c4r2s8 docker-mission % mkdir test          # 폴더 생성
username@c4r2s8 docker-mission % cd test             # 이동
username@c4r2s8 test % touch test.txt                # 빈 파일 생성
username@c4r2s8 test % cp test.txt copy.txt          # 복사
username@c4r2s8 test % mv copy.txt copytest.txt      # 이름 변경
username@c4r2s8 test % cat copytest.txt              # 파일 내용 확인
username@c4r2s8 test % rm copytest.txt               # 삭제
username@c4r2s8 test % ls 
test.txt
```

<details>
    <summary>이미지</summary> 
<p>
<img width="561" height="311" alt="Image" src="https://github.com/user-attachments/assets/fc1718e3-8143-45c0-bda7-3edfd2406c3d" />
</p>
</details>

<br>

### - ✅ 권한 변경 실습

```bash
# test 폴더 권한 변경 전
username@c4r2s8 docker-mission % ls -l
total 0
-rw-r--r--  1 username  groupname   0 Apr  1 09:34 README.md
drwxr-xr-x  3 username  groupname  96 Apr  1 10:15 test

# test 폴더 권한 변경
username@c4r2s8 docker-mission % chmod 744 test

# test 폴더 권한 변경 후
username@c4r2s8 docker-mission % ls -l         
total 0
-rw-r--r--  1 username  groupname   0 Apr  1 09:34 README.md
drwxr--r--  3 username  groupname  96 Apr  1 10:15 test
```

<details>
    <summary>이미지</summary> 
<p>
<img width="540" height="59" alt="Image" src="https://github.com/user-attachments/assets/c9c07e93-928d-42a7-a01c-4e3460c590a6" />
<img width="535" height="71" alt="Image" src="https://github.com/user-attachments/assets/1d971cc1-997f-4f98-8668-f9e4b3cf703c" />
</p>
</details>

<br>

```bash
# text.txt 파일 권한 변경 전
username@c4r2s8 docker-mission % ls -l test    
total 0
-rw-r--r--  1 username  groupname  0 Apr  1 10:13 test.txt

# text.txt 파일 권한 변경
username@c4r2s8 docker-mission % chmod 700 test/test.txt

# text.txt 파일 권한 변경 후
username@c4r2s8 docker-mission % ls -l test             
total 0
-rwx------  1 username  groupname  0 Apr  1 10:13 test.txt
```

<br>

### - ✅ Docker 설치 및 기본 점검

```bash
username@c4r2s8 docker-mission % docker --version
Docker version 28.5.2, build ecc6942
```


```bash
username@c4r2s8 docker-mission % docker info
Client:
 Version:    28.5.2                      # Docker 클라이언트 버전 (사용자가 실행하는 docker 명령어 버전)
 Context:    orbstack                    # 현재 사용 중인 Docker 컨텍스트 
 Debug Mode: false 
 Plugins:
  buildx: Docker Buildx (Docker Inc.)    # 다중 플랫폼 이미지 빌드 플러그인 
    Version:  v0.29.1
    Path:     /Users/username/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)  # 다중 컨테이너 정의 및 실행 플러그인 
    Version:  v2.40.3
    Path:     /Users/username/.docker/cli-plugins/docker-compose

Server:
 Containers: 0                           # 현재 존재하는 전체 컨테이너 개수 
  Running: 0                             # 실행 중인 컨테이너 개수 
  Paused: 0                              # 일시 중지된 컨테이너 개수 
  Stopped: 0                             # 중지된 컨테이너 개수 
 Images: 0                               # 현재 저장된 Docker 이미지 개수
 Server Version: 28.5.2
 Storage Driver: overlay2                # 컨테이너 파일 시스템 드라이버 (overlay2 = 레이어 기반 저장소)
  Backing Filesystem: btrfs 
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file               # 컨테이너 로그 저장 방식 (json 파일 형식)
 Cgroup Driver: cgroupfs                 # 리소스 제한 관리 드라이버 
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive                         # Docker Swarm 클러스터 모드 상태 (inactive = 단일 호스트 모드)
 Runtimes: io.containerd.runc.v2 runc    # 컨테이너 실행 엔진 목록 (runc = 표준 OCI 런타임)
 Default Runtime: runc                   # 기본으로 사용할 컨테이너 실행 엔진 
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: ...
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6                                 # Docker에 할당된 CPU 코어 수
 Total Memory: 15.67GiB                  # Docker에 할당된 메모리 크기 
 Name: ...
 ID: ...
 Docker Root Dir: ...
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false             # Docker 데몬 재시작 시 컨테이너 유지 여부 
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```

<details>
    <summary>이미지 모아보기</summary> 
<p>
<img width="420" height="30" alt="Image" src="https://github.com/user-attachments/assets/3b2c2799-4222-4ed8-bcd6-e5fc97d26361" />
</p>
<p>
<img width="529" height="574" alt="Image" src="https://github.com/user-attachments/assets/9b80d897-2170-4f76-9d95-8c91080a53e9" />
</p>
</details>

<br>

### - ✅ 컨테이너 실행 실습

hello-world 실행 

```bash
username@c4r2s8 docker-mission % docker run hello-world             # hello-world 이미지가 없으면 Docker Hub에서 다운로드
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:452a468a4bf985040037cb6d5392410206e47db9bf5b7278d281f94d1c2d0931
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

<br>

ubuntu 컨테이너 실행 후 내부 진입

```bash 
username@c4r2s8 docker-mission % docker run -it ubuntu bash        # 내 컴퓨터 안에 리눅스 운영체제를 하나 더 띄움 
Unable to find image 'ubuntu:latest' locally                       # -i (표준입력 활성화), -t (화면에 입출력을 보여줌)
latest: Pulling from library/ubuntu
817807f3c64e: Pull complete 
Digest: sha256:186072bba1b2f436cbb91ef2567abca677337cfc786c86e107d25b7072feef0c
Status: Downloaded newer image for ubuntu:latest
root@c7c94027e58a:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@c7c94027e58a:/# echo hello
hello
```

<details>
    <summary>이미지 모아보기</summary> 
<p>
<img width="568" height="412" alt="Image" src="https://github.com/user-attachments/assets/6f8565b9-f418-40d1-8ae8-5f77d7008054" />
</p>
<p>
<img width="569" height="189" alt="Image" src="https://github.com/user-attachments/assets/28b25b49-0814-4196-811d-8b63ad5d1007" />
</p>
</details>

<br>

컨테이너 종료/유지 차이 

```bash
root@c7c94027e58a:/# exit
exit

# 컨테이너를 뒤에서 실행
username@c4r2s8 docker-mission % docker run -itd --name my-test ubuntu bash     # -d (detach) 
f4e8197b5e8d856e9d81befb8e20b0d88ca77cbbe677dd91ce5cff7e6982ae95

# 현재 실행 중인 컨테이너 확인
username@c4r2s8 docker-mission % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
f4e8197b5e8d   ubuntu    "bash"    9 seconds ago   Up 9 seconds             my-test

# attach 실습: 실행 중인 컨테이너의 메인 프로세스(bash)에 직접 연결
username@c4r2s8 docker-mission % docker attach my-test
root@f4e8197b5e8d:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@f4e8197b5e8d:/# exit
exit

# attach는 컨테이너의 메인 프로세스에 직접 붙는 거라, 내가 나오면 컨테이너도 같이 죽음 
username@c4r2s8 docker-mission % docker ps                         # 실행 중인 목록에 my-test 없음 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

username@c4r2s8 docker-mission % docker start my-test
my-test
username@c4r2s8 docker-mission % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS         PORTS     NAMES
f4e8197b5e8d   ubuntu    "bash"    About a minute ago   Up 4 seconds             my-test

# exec 실습: 실행 중인 컨테이너에 별도의 새로운 bash 프로세스를 실행하여 접속 
username@c4r2s8 docker-mission % docker exec -it my-test bash
root@f4e8197b5e8d:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@f4e8197b5e8d:/# exit
exit

# exec로 띄운 프로세스만 종료되었으므로, 컨테이너 본체는 여전히 살아있음
username@c4r2s8 docker-mission % docker ps                         # 종료해서 나왔는데도 my-test 컨테이너가 살아 있음 
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS          PORTS     NAMES
f4e8197b5e8d   ubuntu    "bash"    About a minute ago   Up 28 seconds             my-test
```

<br>

attach vs exec 차이점

- attach: 실행 중인 컨테이너에 접속. exit 시 컨테이너도 함께 종료됨
- exec: 실행 중인 컨테이너에 새로운 프로세스를 실행. exit 해도 컨테이너는 계속 실행됨

<img width="639" height="410" alt="Image" src="https://github.com/user-attachments/assets/e5955b0f-0cf8-490a-8328-4638537751d9" />
<br>
<br>

### - ✅ Docker 기본 운영 명령 수행
이미지: 다운로드/목록 확인

```bash
username@c4r2s8 docker-mission % docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    e2ac70e7319a   8 days ago    10.1kB
ubuntu        latest    f794f40ddfff   5 weeks ago   78.1MB
```

컨테이너 실행/중지/목록 확인 

```bash
# -all (가동 중, 멈춘 컨테이너를 모두 다 보여줌)
username@c4r2s8 docker-mission % docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED       STATUS                     PORTS     NAMES
f4e8197b5e8d   ubuntu        "bash"     3 hours ago   Up 3 hours                           my-test
c7c94027e58a   ubuntu        "bash"     3 hours ago   Exited (127) 3 hours ago             nostalgic_euclid
25119f6689f7   hello-world   "/hello"   3 hours ago   Exited (0) 2 hours ago               youthful_heisenberg

username@c4r2s8 docker-mission % docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS       PORTS     NAMES
f4e8197b5e8d   ubuntu    "bash"    3 hours ago   Up 3 hours             my-test

# 컨테이너 중지 
username@c4r2s8 docker-mission % docker stop my-test 
my-test

username@c4r2s8 docker-mission % docker ps -a        
CONTAINER ID   IMAGE         COMMAND    CREATED       STATUS                       PORTS     NAMES
f4e8197b5e8d   ubuntu        "bash"     3 hours ago   Exited (137) 7 seconds ago             my-test
c7c94027e58a   ubuntu        "bash"     3 hours ago   Exited (127) 3 hours ago               nostalgic_euclid
25119f6689f7   hello-world   "/hello"   3 hours ago   Exited (0) 3 hours ago                 youthful_heisenberg
```

<img width="788" height="111" alt="Image" src="https://github.com/user-attachments/assets/0520eebd-720a-45a5-88ca-1494121c9beb" />
<img width="798" height="98" alt="Image" src="https://github.com/user-attachments/assets/ca91c639-84b3-41dc-9275-a885af4adb41" />


운영: 로그 확인, 리소스 확인 

로그 확인

```bash
username@c4r2s8 docker-mission % docker logs my-test
root@f4e8197b5e8d:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@f4e8197b5e8d:/# exit
exit
root@f4e8197b5e8d:/# %   
```

<img width="435" height="103" alt="Image" src="https://github.com/user-attachments/assets/d163c341-9d52-4831-8c76-a552790a596f" />

리소스 확인
```bash
# my-test 실행 중
$ docker stats 

CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS 
f4e8197b5e8d   my-test   0.00%     1.801MiB / 15.67GiB   0.01%     1.13kB / 126B   4.45MB / 0B   1 
```

<img width="727" height="78" alt="Image" src="https://github.com/user-attachments/assets/1708154d-0826-45a6-baef-48a9eaf7b044" />

<br>

### - ✅ 기존 Dockerfile 기반 커스텀 이미지 제작

사용한 베이스 이미지 : nginx:alpine

선택 이유 : 별도의 웹 서버 설치 과정이 필요 없어서 

내가 적용한 커스텀 포인트 : index.html 정적 파일을 nginx 기본 경로에 복사하여 기본 페이지를 대체함 

<br>

Dockerfile

<p>
    <img width="679" height="119" alt="Image" src="https://github.com/user-attachments/assets/450fe166-c029-4a5e-885c-dce09b6e431c" />
</p>

<br>

index.html

<p>
    <img width="703" height="125" alt="Image" src="https://github.com/user-attachments/assets/80755a7d-2b5c-4186-aa05-9a2d4218f7c7" />
</p>

<br>

```bash 
username@c4r2s8 docker-mission % ls
README.md	test
username@c4r2s8 docker-mission % touch Dockerfile
username@c4r2s8 docker-mission % touch test/index.html
username@c4r2s8 docker-mission % nano test/index.html
username@c4r2s8 docker-mission % nano Dockerfile 

username@c4r2s8 docker-mission % ls
Dockerfile	README.md	test
username@c4r2s8 docker-mission % docker build -t test-image:v1 .
[+] Building 7.6s (7/7) FINISHED                                                                        docker:orbstack
 => [internal] load build definition from Dockerfile                                                               0.2s
 => => transferring dockerfile: 87B                                                                                0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                    2.8s
 => [internal] load .dockerignore                                                                                  0.1s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load build context                                                                                  0.2s
 => => transferring context: 169B                                                                                  0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34c  3.7s
 => => resolve docker.io/library/nginx:alpine@sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34c  0.2s
 => => sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34cf6d1 10.33kB / 10.33kB                   0.0s
 => => sha256:7e89aa6cabfc80f566b1b77b981f4bb98413bd2d513ca9a30f63fe58b4af6903 2.50kB / 2.50kB                     0.0s
 => => sha256:d5030d429039a823bef4164df2fad7a0defb8d00c98c1136aec06701871197c2 12.32kB / 12.32kB                   0.0s
 => => sha256:589002ba0eaed121a1dbf42f6648f29e5be55d5c8a6ee0f8eaa0285cc21ac153 3.86MB / 3.86MB                     0.5s
 => => sha256:8892f80f46a05d59a4cde3bcbb1dd26ed2441d4214870a4a7b318eaa476a0a54 1.87MB / 1.87MB                     0.8s
 => => sha256:91d1c9c22f2c631288354fadb2decc448ce151d7a197c167b206588e09dcd50a 626B / 626B                         0.9s
 => => extracting sha256:589002ba0eaed121a1dbf42f6648f29e5be55d5c8a6ee0f8eaa0285cc21ac153                          0.1s
 => => sha256:cf1159c696ee2a72b85634360dbada071db61bceaad253db7fda65c45a58414c 953B / 953B                         1.0s
 => => extracting sha256:8892f80f46a05d59a4cde3bcbb1dd26ed2441d4214870a4a7b318eaa476a0a54                          0.1s
 => => sha256:3f4ad4352d4f91018e2b4910b9db24c08e70192c3b75d0d6fff0120c838aa0bb 402B / 402B                         1.3s
 => => extracting sha256:91d1c9c22f2c631288354fadb2decc448ce151d7a197c167b206588e09dcd50a                          0.0s
 => => sha256:c2bd5ab177271dd59f19a46c214b1327f5c428cd075437ec0155ae71d0cdadc1 1.21kB / 1.21kB                     1.4s
 => => extracting sha256:cf1159c696ee2a72b85634360dbada071db61bceaad253db7fda65c45a58414c                          0.0s
 => => sha256:4d9d41f3822d171ccc5f2cdfd75ad846ac4c7ed1cd36fb998fe2c0ce4501647b 1.40kB / 1.40kB                     1.6s
 => => extracting sha256:3f4ad4352d4f91018e2b4910b9db24c08e70192c3b75d0d6fff0120c838aa0bb                          0.0s
 => => sha256:3370263bc02adcf5c4f51831d2bf1d54dbf9a6a80b0bf32c5c9b9400630eaa08 20.25MB / 20.25MB                   2.0s
 => => extracting sha256:c2bd5ab177271dd59f19a46c214b1327f5c428cd075437ec0155ae71d0cdadc1                          0.0s
 => => extracting sha256:4d9d41f3822d171ccc5f2cdfd75ad846ac4c7ed1cd36fb998fe2c0ce4501647b                          0.0s
 => => extracting sha256:3370263bc02adcf5c4f51831d2bf1d54dbf9a6a80b0bf32c5c9b9400630eaa08                          0.4s
 => [2/2] COPY test /usr/share/nginx/html                                                                          0.2s
 => exporting to image                                                                                             0.2s
 => => exporting layers                                                                                            0.1s
 => => writing image sha256:d65086d4cc339246f5ed77567ab64fcb5a8aad91d344eedba9508f15d73c0008                       0.0s
 => => naming to docker.io/library/test-image:v1                                                                   0.0s
username@c4r2s8 docker-mission % docker images
REPOSITORY    TAG       IMAGE ID       CREATED              SIZE
test-image    v1        d65086d4cc33   About a minute ago   62.2MB
hello-world   latest    e2ac70e7319a   8 days ago           10.1kB
ubuntu        latest    f794f40ddfff   5 weeks ago          78.1MB
```
<br>

<p>
    <img width="1287" height="972" alt="Image" src="https://github.com/user-attachments/assets/f9be1540-6d47-4df7-b8a4-63fd9f32a0ce" />
</p>

<br>

### - ✅ 포트 매핑

포트 매핑 설정 방식: -p 8080:80
<Br>
호스트의 8080 포트를 컨테이너 내부 Nginx 기본 포트인 80에 연결

방식: 바인드 마운트 (-v $(pwd)/test:/usr/share/nginx/html)

- 별도의 이미지 빌드 없이도 로컬의 test 폴더 내 소스 수정만으로 즉각적인 환경 재현이 가능
- 컨테이너가 삭제되어도 test/index.html 데이터가 유실되지 않고 호스트에 보존됨 

[포트 매핑 접속 증거](#5-1-웹-페이지-한글-깨짐)

<br>

### - ✅ 바인드 마운트 반영

```bash
username@c4r2s8 docker-mission % docker rm -f web-test               # 기존 컨테이너 삭제 
web-test
username@c4r2s8 docker-mission % docker run -d -p 8080:80 -v $(pwd)/test:/usr/share/nginx/html --name web-bind nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
589002ba0eae: Already exists 
8892f80f46a0: Already exists 
91d1c9c22f2c: Already exists 
cf1159c696ee: Already exists 
3f4ad4352d4f: Already exists 
c2bd5ab17727: Already exists 
4d9d41f3822d: Already exists 
3370263bc02a: Already exists 
Digest: sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34cf6d1
Status: Downloaded newer image for nginx:alpine
61dcacba7dbbda2d5b20174e44cca1355f50b630fc2fbf80794fc333f668f524
username@c4r2s8 docker-mission % nano test/index.html 
```

<br>

<변경 전>

<p>
<img width="1040" height="392" alt="Screenshot 2026-04-02 at 10 31 32 AM" src="https://github.com/user-attachments/assets/91c0d077-54a2-4ada-8de6-d33ba2110af0" />
</p>

<br>

<변경 후>

<p>
<img width="846" height="403" alt="Screenshot 2026-04-02 at 10 32 56 AM" src="https://github.com/user-attachments/assets/ebb4cafe-0fad-4bbd-9b1a-d55f29599e28" />
</p>

바인드 마운트로 호스트의 test 폴더와 컨테이너의 nginx 웹 경로를 연결
<br>
-> index.html 수정 후 별도의 이미지 빌드나 재시작 없이도, 브라우저 새로고침만 하면 변경 사항이 반영됨 

<br>

### - ✅ <span id="volume-test">Docker 볼륨 영속성 검증</span>

```bash
# 현재 생성되어 있는 볼륨 없음 
username@c4r2s8 docker-mission % docker volume ls
DRIVER    VOLUME NAME
username@c4r2s8 docker-mission % docker volume create my-volume            # 볼륨 생성 
my-volume

# 컨테이너 실행 (볼륨 연결)
username@c4r2s8 docker-mission % docker run -d -p 8081:80 -v my-volume:/usr/share/nginx/html --name test-volume nginx   
61e55126cb112da59192eeb3036ffbb36c91617afea880e3327710ea6b25aaff

# 컨테이너 안에 파일 생성 
username@c4r2s8 docker-mission % docker exec test-volume sh -c "echo 'hello volume' > /usr/share/nginx/html/index.html"

# 컨테이너 삭제 
username@c4r2s8 docker-mission % docker rm -f test-volume
test-volume

# 새로운 컨테이너 실행 (같은 볼륨 연결)
username@c4r2s8 docker-mission % docker run -d -p 8081:80 -v my-volume:/usr/share/nginx/html --name test-volume2 nginx
a56ebea60d55642e1d390a4d49d7f9c6893ae1ec25e1241805e330d04a6d1baf
```

<img width="743" height="276" alt="Screenshot 2026-04-02 at 1 16 54 PM" src="https://github.com/user-attachments/assets/17202b85-a91e-40f4-b429-e2d1a49ed2fc" />

<br>

## 4. 학습한 내용 
### 4-1. 리눅스 파일 권한

예) drwxr-xr-x

| 구분 | 타입 | 소유자 (User) | 그룹 (Group) | 그 외 (Others) |
| :---: | :---: | :---: | :---: | :---: |
| 표기 | d | rwx | r-x | r-x |

- 타입 : d는 디렉토리, -는 일반 파일을 의미
- 그룹 : 소유자가 속한 그룹 멤버들의 권한

<br>

| 권한 | 파일(File)에서의 의미 | 디렉토리(Directory)에서의 의미 |
| :---: | :---: | :---: |
| r (read) | 파일의 내용을 읽기 (cat, vi 등) | 디렉토리 내부의 파일 목록 보기 (ls) |
| w (write)	| 파일의 내용을 수정	| 파일/디렉토리 생성, 삭제, 이름 변경 |
| x (execute) |	파일을 프로그램으로 실행 | 디렉토리 내부로 진입 (cd) |

<br>

### 4-2. 도커 데몬
도커면 도커지.. 도대체 What is 도커 데몬?
docker run ... 을 쳤을 때, 명령어를 실행하는 건 터미널이 아니라, 뒤에서 대기하고 있던 도커 데몬(이미지를 찾아서 컨테이너에 띄우는 역할을 수행)임

<br>

### 4-3. docker images
| 컬럼 | 설명 | 예시 |
|:---:|:---:|:---:|
| **REPOSITORY** | 이미지의 이름 | hello-world, ubuntu |
| **TAG** | 이미지의 버전/태그 | latest (최신 버전) |
| **IMAGE ID** | 이미지의 고유 ID | e2ac70e7319a |
| **CREATED** | 이미지 생성 시간 | 8 days ago |
| **SIZE** | 이미지 용량 | 10.1kB |

<br>

### 4-4. 볼륨 영속성
컨테이너가 삭제되어도 데이터가 살아있게 만드는 것

| 구분 | 볼륨 | 바인드 마운트 |
|:---:|:---:|:---:|
| **위치** | Docker가 관리 | 내 로컬 디렉토리 |
| **사용 목적** | 데이터 영속성 | 개발 편의성 |
| **개념** | Docker 전용 저장소 | 내 폴더를 연결 |

- Docker 컨테이너는 기본적으로 일회용
- 컨테이너 삭제하면 내부 데이터도 같이 날아가기 때문에 데이터를 따로 저장하는 공간인 Volume이 필요
- (컨테이너와 데이터 저장소를 분리)

<br>

### 4-5. 이미지 VS 컨테이너

| 구분 | 이미지 | 컨테이너 |
|:---:|:---:|:---:|
| **상태** | 실행되지 않은 정적 파일 (설계도) | 메모리에서 실행 중인 프로세스 (실체) |
| **레이어** | Read-Only (읽기 전용) 레이어들의 집합 | 이미지 위에 Writable (쓰기 가능) 레이어 추가 |
| **비유** | 설치 프로그램 (.exe), 요리 레시피 | 실행된 프로그램, 실제로 만들어진 요리 |

<br>

"빌드 / 실행 / 변경" 관점에서의 차이
1) 빌드 관점
   - 이미지 : Dockerfile에 정의된 명령어를 실행하여 생성됨. 소스 코드, 라이브러리, 환경 설정이 불변(immutable)의 레이어로 쌓여 하나의 파일 덩어리가 됨
   - 컨테이너 : 빌드 단계에서는 존재하지 않음
2) 실행 관점
   - 이미지 : 스스로 실행될 수 없음. 컨테이너를 띄우기 위한 재료. 하나의 이미지로 수백개의 똑같은 컨테이너를 실행할 수 있음
   - 컨테이너 : docker run을 통해 이미지를 바탕으로 격리된 환경이 생성됨.
3) 변경 관점
   - 이미지 : 한번 생성되면 절대 변경할 수 없음. 소스 코드를 수정했다면 새로운 이미지를 다시 빌드해야 함
   - 컨테이너 : 실행 중에 내부 파일을 수정하거나 설정을 바꿀 수 있음(이미지 위에 얹어진 컨테이너 레이어 writable layer 에 기록되기 때문). 하지만 컨테이너를 삭제하면 사라지므로 볼륨이나 바인드 마운트가 필요함

<br>

### 4-6. 컨테이너 포트 접속 및 매핑의 이론적 배경

Q. 컨테이너 내부 포트로 직접 접속할 수 없는 이유는?
- 도커 컨테이너는 호스트 OS와 분리된 별도의 가상 네트워크 환경(Docker Bridge) 내에서 실행됨
- 컨테이너는 도커가 생성한 격리된 가상 네트워크 안에서 별도의 사설 IP를 할당받아 동작함. 호스트 OS 외부에서는 이 가상 네트워크의 내부 주소를 알 수 없으므로 직접적인 통신이 불가능

Q. 포트 매핑이 필요한 이유는?
- 외부 네트워크(호스트 브라우저 등)와 격리된 컨테이너 네트워크 사이의 통로(Bridge)를 만들기 위해서
- 포트 충돌 방지를 위해(하나의 호스트에서 똑같은 80 포트를 쓰는 웹 서버 컨테이너를 여러 개 띄우더라도, 호스트 쪽 포트를 각각 8081:80, 8082:80으로 다르게 매핑하면 충돌 없이 동시에 서비스 가능/집 주소는 하나지만, 들어가는 현관문 번호는 여러 개가 가능한 것처럼)

<br>

### 4-7. 절대 경로 VS 상대 경로 

  
<br>

## 5. 트러블슈팅 해결 
### 5-1. 웹 페이지 한글 깨짐 
#### - 문제 상황
index.html 파일에 한글을 작성한 후 http://localhost:8080으로 접속했을 때 한글이 깨진 문자로 표시되었음

#### - 원인 가설 1
브라우저 문자 인코딩이 UTF-8로 설정되지 않아 발생한 문제로 판단함

#### - 확인 과정 1
index.html에 따로 문자 인코딩을 해주는 부분이 없음

#### - 해결 시도 1
index.html 파일의 <head> 태그 내부에 <meta charset="UTF-8"> 를 추가함 

<br>

<변경 전>

<p>
    <img width="485" height="113" alt="Screenshot 2026-04-02 at 8 58 33 AM" src="https://github.com/user-attachments/assets/6eea1316-d74d-4c39-bdd1-d98beb1e6460" />
</p>

<br>

<변경 후>

<p>
    <img width="533" height="221" alt="Screenshot 2026-04-02 at 9 08 35 AM" src="https://github.com/user-attachments/assets/57b33c89-47fe-4979-80e6-9cec8f456536" />
</p>

<br>

docker start my-web 실행 

```bash
username@c4r2s8 ~ % cd docker-mission 
username@c4r2s8 docker-mission % ls
Dockerfile	README.md	test
username@c4r2s8 docker-mission % nano test/index.html 
username@c4r2s8 docker-mission % nano test/index.html
username@c4r2s8 docker-mission % docker ps -a
CONTAINER ID   IMAGE           COMMAND                  CREATED        STATUS                      PORTS     NAMES
5bb4e94f56ea   test-image:v1   "/docker-entrypoint.…"   14 hours ago   Exited (0) 14 hours ago               web-test
f4e8197b5e8d   ubuntu          "bash"                   18 hours ago   Exited (137) 14 hours ago             my-test
c7c94027e58a   ubuntu          "bash"                   18 hours ago   Exited (127) 18 hours ago             nostalgic_euclid
25119f6689f7   hello-world     "/hello"                 18 hours ago   Exited (0) 18 hours ago               youthful_heisenberg
username@c4r2s8 docker-mission % docker start web-test
web-test
```

<br>

#### - 결과
그러나 한글 깨짐 현상이 여전히 해결되지 않았음

<p>
    <img width="986" height="454" alt="Screenshot 2026-04-02 at 9 19 52 AM" src="https://github.com/user-attachments/assets/17fdb930-d967-41af-835e-64ee7d693ad1" />
</p>

<br>

#### - 원인 가설 2
실행 중인 컨테이너는 수정 전의 index.html을 품고 있는 예전 이미지로 만들어진 상태라서, 수정된 html이 반영되지 않은 것으로 판단함

#### - 확인 과정 2
docker images로 이미지 생성 시간을 확하였는데 예전 시간 그대로였음 

#### - 해결 방법
컨테이너를 삭제한 후 이미지를 다시 빌드하고 컨테이너를 실행함

```bash
username@c4r2s8 docker-mission % docker rm -f web-test
web-test
username@c4r2s8 docker-mission % docker build -t test-image:v1
ERROR: docker: 'docker buildx build' requires 1 argument

Usage:  docker buildx build [OPTIONS] PATH | URL | -

Run 'docker buildx build --help' for more information
username@c4r2s8 docker-mission % docker build -t test-image:v1 .
[+] Building 2.7s (7/7) FINISHED                                docker:orbstack
 => [internal] load build definition from Dockerfile                       0.1s
 => => transferring dockerfile: 87B                                        0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine            1.8s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.0s
 => [internal] load build context                                          0.1s
 => => transferring context: 312B                                          0.0s
 => CACHED [1/2] FROM docker.io/library/nginx:alpine@sha256:e7257f1ef28ba  0.0s
 => [2/2] COPY test /usr/share/nginx/html                                  0.2s
 => exporting to image                                                     0.2s
 => => exporting layers                                                    0.1s
 => => writing image sha256:0dfb292e2132cf8ca0cdd39c52a5c6ff8e8159485f790  0.0s
 => => naming to docker.io/library/test-image:v1                           0.0s
username@c4r2s8 docker-mission % docker run -d -p 8080:80 --name web-test
docker: 'docker run' requires at least 1 argument

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

See 'docker run --help' for more information
username@c4r2s8 docker-mission % docker run -d -p 8080:80 --name web-test test-image:v1
a8703d4da0c44175bcfaa9f411a3b485daa0de8ff463f9fdac8d6aad9eff9f47
```

<p>
    <img width="1056" height="631" alt="Screenshot 2026-04-02 at 9 29 53 AM" src="https://github.com/user-attachments/assets/c818a21f-8a45-42b3-81c6-7d4ccf9b5491" />
</p>

<br>

### 5-2. 포트 충돌로 인한 컨테이너 실행 실패

```bash 
username@c4r2s8 docker-mission % docker run -d -p 8080:80 -v my-volume:/usr/share/nginx/html --name test-container nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
ec781dee3f47: Pull complete 
bb3d0aa29654: Pull complete 
510ddf6557d6: Pull complete 
cde7a05ae428: Pull complete 
587e3d84dbb5: Pull complete 
3189680c601f: Pull complete 
5e815e07e569: Pull complete 
Digest: sha256:7150b3a39203cb5bee612ff4a9d18774f8c7caf6399d6e8985e97e28eb751c18
Status: Downloaded newer image for nginx:latest
90213b89a0cfdc20fd5b27f530a47994dbcd04dee84ac7db32a8492c2604a29e
docker: Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint test-container (bbe0d2ff64ccdb3fafec89103a813396a1c8423914033fc1f8abb483de1b0ba7): Bind for 0.0.0.0:8080 failed: port is already allocated

Run 'docker run --help' for more information

username@c4r2s8 docker-mission % docker exec test-container sh -c "echo 'hello volume' > /usr/share/nginx/html/index.html"
Error response from daemon: container 90213b89a0cfdc20fd5b27f530a47994dbcd04dee84ac7db32a8492c2604a29e is not running
username@c4r2s8 docker-mission % docker run -d -p 8081:80 -v my-volume:/usr/share/nginx/html --name test-container nginx
docker: Error response from daemon: Conflict. The container name "/test-container" is already in use by container "90213b89a0cfdc20fd5b27f530a47994dbcd04dee84ac7db32a8492c2604a29e". You have to remove (or rename) that container to be able to reuse that name.

Run 'docker run --help' for more information
```

#### - 문제 상황
컨테이너 실행에 실패하고, 컨테이너 내부에 파일을 생성하려고 해도 오류가 뜸 

#### - 원인 가설
이미 다른 컨테이너가 호스트의 8080 포트를 사용 중이어서 포트 바인딩에 실패하고, 실행 중이지 않은 컨테이너 내부에 파일을 생성하려고 해서 실패했을 것이라고 판단함 

#### - 확인 과정 
docker ps로 8080 포트를 사용 중인 기존 컨테이너 확인 

<p>
    <img width="560" height="73" alt="Screenshot 2026-04-02 at 1 27 40 PM" src="https://github.com/user-attachments/assets/de59d986-039e-4926-b328-be7feab2fb9d" />
</p>

#### - 해결 방법 
다른 포트로 컨테이너 실행함 

[결과 화면](#volume-test)

