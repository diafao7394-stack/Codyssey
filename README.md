# Codyssey

1) 실행환경
- OS: Windows 11 
- Shell: PowerShell 
- Docker: 29.9.2
- Git: 2.45.2.windows.1


2) 수행 체크리스트

-[O] 터미널 조작 로그 기록
-[] 권한 실습 및 증거 기록
-[] Docker 설치 및 기본 점검
-[] Docker 기본 운영 명령 수행
-[] 컨테이너 실행 실습
-[] 기존 Dockerfile 기반 커스텀 이미지 제작
-[] 포트 매핑 및 접속 증거
-[] Docker 볼륨 영속성 검증
-[] Git 설정 및 GitHub 연동


터미널 조작 로그 기록

(현재 위치 확인)
PS C:\Users\difao\.codyssey> pwd
Path
----
C:\Users\difao\.codyssey

목록 확인(숨김 파일 포함)
PS C:\Users\difao\.codyssey> ls -Force
    디렉터리: C:\Users\difao\.codyssey
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-10-30  오전 10:58                a
d-----      2026-10-30  오전 10:58                b
-a----      2026-10-30  오전 10:58              0 ex1.txt

현재 위치 이동하기 
PS C:\Users\difao\.codyssey> cd a
PS C:\Users\difao\.codyssey\a> cd ..
PS C:\Users\difao\.codyssey>

빈 파일 생성
PS C:\Users\difao\.codyssey> ni empty.txt -ItemType File
    디렉터리: C:\Users\difao\.codyssey
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-10-30  오전 11:03              0 empty.txt

파일 복사
PS C:\Users\difao\.codyssey> cp empty.txt c.txt

파일 이동
PS C:\Users\difao\.codyssey> mv empty.txt "C:\Users\difao\.codyssey\a"
PS C:\Users\difao\.codyssey> cd a
PS C:\Users\difao\.codyssey\a> ls
    디렉터리: C:\Users\difao\.codyssey\a
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-10-30  오전 11:05              8 empty.txt

파일 이름 변경
PS C:\Users\difao\.codyssey\a> ren empty.txt d.txt
PS C:\Users\difao\.codyssey\a> ls
    디렉터리: C:\Users\difao\.codyssey\a
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-10-30  오전 11:05              8 d.txt

파일 삭제
PS C:\Users\difao\.codyssey\a> rm d.txt
PS C:\Users\difao\.codyssey\a> ls  (하나 있던 파일을 삭제하여서 ls를 입력했을 때 출력값이 없다.)
PS C:\Users\difao\.codyssey\a>

파일 내용 확인
PS C:\Users\difao\.codyssey> cat c.txt
asdfasdf

빈 폴더 생성
PS C:\Users\difao\.codyssey> mkdir aa
    디렉터리: C:\Users\difao\.codyssey
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-10-30  오전 11:15                aa
PS C:\Users\difao\.codyssey> ls
    디렉터리: C:\Users\difao\.codyssey
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----      2026-10-30  오전 11:11                a
d-----      2026-10-30  오전 11:15                aa
d-----      2026-10-30  오전 10:58                b
-a----      2026-10-30  오전 11:05              8 c.txt
-a----      2026-10-30  오전 10:58              0 ex1.tx
