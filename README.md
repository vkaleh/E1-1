# AI/SW 워크스테이션 구축

## 1. 프로젝트 개요

## 2. 실행 환경
-OS :

## 3. 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
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

</br>
<img width="561" height="311" alt="Image" src="https://github.com/user-attachments/assets/27139d10-5684-4e49-a532-85b7e6d44d7e" />
</br>

- [x] 권한 변경 실습

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

<img width="540" height="59" alt="Image" src="https://github.com/user-attachments/assets/c9c07e93-928d-42a7-a01c-4e3460c590a6" />
<img width="535" height="71" alt="Image" src="https://github.com/user-attachments/assets/1d971cc1-997f-4f98-8668-f9e4b3cf703c" />
</br>

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

- [x] Docker 설치 및 기본 점검

```bash
username@c4r2s8 docker-mission % docker --version
Docker version 28.5.2, build ecc6942
```

<img width="1060" height="58" alt="image" src="https://github.com/user-attachments/assets/dc254222-d8ec-4eb4-88a6-ae7a655801b7" />


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

<img width="1057" height="1148" alt="Image" src="https://github.com/user-attachments/assets/0147c951-71bd-4ae9-8e01-907ff16c6edb" />

- [ ] Docker 기본 운영 명령 수행
- [ ] 컨테이너 실행 실습
- [ ] 기존 Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑
- [ ] 바인드 마운트 반영
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동

## 4. 학습한 내용 
4-1. 리눅스 파일 권한

예) drwxr-xr-x

| 구분 | 타입 | 소유자 (User) | 그룹 (Group) | 그 외 (Others) |
| :---: | :---: | :---: | :---: | :---: |
| 표기 | d | rwx | r-x | r-x |

- 타입 : d는 디렉토리, -는 일반 파일을 의미
- 그룹 : 소유자가 속한 그룹 멤버들의 권한

</br>

| 권한 | 파일(File)에서의 의미 | 디렉토리(Directory)에서의 의미 |
| :---: | :---: | :---: |
| r (read) | 파일의 내용을 읽기 (cat, vi 등) | 디렉토리 내부의 파일 목록 보기 (ls) |
| w (write)	| 파일의 내용을 수정	| 파일/디렉토리 생성, 삭제, 이름 변경 |
| x (execute) |	파일을 프로그램으로 실행 | 디렉토리 내부로 진입 (cd) |

