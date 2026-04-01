# AI/SW 워크스테이션 구축

## 1. 프로젝트 개요

## 2. 실행 환경
-OS :

## 3. 수행 체크리스트
- [x] 터미널 기본 조작 및 폴더 구성
```bash
loveangelina6012@c4r2s8 docker-mission % pwd                 # 현재 위치 확인
/Users/loveangelina6012/docker-mission
loveangelina6012@c4r2s8 docker-mission % ls -la              # 파일 목록 확인 (숨긴 파일 포함)
total 0
drwxr-xr-x   4 loveangelina6012  loveangelina6012  128 Apr  1 09:34 .
drwxr-x---+ 22 loveangelina6012  loveangelina6012  704 Apr  1 09:41 ..
drwxr-xr-x  12 loveangelina6012  loveangelina6012  384 Apr  1 09:53 .git
-rw-r--r--   1 loveangelina6012  loveangelina6012    0 Apr  1 09:34 README.md
loveangelina6012@c4r2s8 docker-mission % mkdir test          # 폴더 생성
loveangelina6012@c4r2s8 docker-mission % cd test             # 이동
loveangelina6012@c4r2s8 test % touch test.txt                # 빈 파일 생성
loveangelina6012@c4r2s8 test % cp test.txt copy.txt          # 복사
loveangelina6012@c4r2s8 test % mv copy.txt copytest.txt      # 이름 변경
loveangelina6012@c4r2s8 test % cat copytest.txt              # 파일 내용 확인
loveangelina6012@c4r2s8 test % rm copytest.txt               # 삭제
loveangelina6012@c4r2s8 test % ls 
test.txt
```

- [ ] 권한 변경 실습
- [ ] Docker 설치 및 기본 점검
- [ ] Docker 기본 운영 명령 수행
- [ ] 컨테이너 실행 실습
- [ ] 기존 Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑
- [ ] 바인드 마운트 반영
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동

