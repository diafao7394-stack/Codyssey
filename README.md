# Codyssey

1) 실행환경
- OS: Windows 11 
- Shell: PowerShell 
- Docker: 29.9.2
- Git: 2.45.2.windows.1


2) 수행 체크리스트

-[O] 터미널 조작 로그 기록
-[O] 권한 실습 및 증거 기록
-[O] Docker 설치 및 기본 점검
-[O] Docker 기본 운영 명령 수행
-[O] 컨테이너 실행 실습
-[] 기존 Dockerfile 기반 커스텀 이미지 제작
-[] 포트 매핑 및 접속 증거
-[] Docker 볼륨 영속성 검증
-[] Git 설정 및 GitHub 연동


1. 터미널 조작 로그 기록

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

권한 실습 및 증거 기록.
권한을 확인/변경하는 명령을 수행하고 변경 전/후 비교를 기술 문서에 남긴다.
최소 요구 : 파일 1개, 디렉토리 1개에 대해 권한 변경 실험을 수행한다.
icacls는 파일이나 폴더의 접근 권한(ACL)을 확인하거나 변경하는 명령어이다.
icacls = Integrity Control Access Control List

PS C:\Users\difao\.codyssey> $lab = ".\권한실습"
PS C:\Users\difao\.codyssey> mkdir "$lab\폴더"
PS C:\Users\difao\.codyssey> Set-Content "$lab\파일.txt" "실습용 내용"
PS C:\Users\difao\.codyssey> $me = whoami
PS C:\Users\difao\.codyssey> icacls "$lab\파일.txt"
PS C:\Users\difao\.codyssey> icacls "$lab\파일.txt" /deny "${me}:w"
PS C:\Users\difao\.codyssey> icacls "$lab\파일.txt"
.\권한실습\파일.txt BOOK-P2FAS03CTM\difao:(DENY)(W)
              NT AUTHORITY\SYSTEM:(I)(F)
              BUILTIN\Administrators:(I)(F)
              BOOK-P2FAS03CTM\difao:(I)(F)

PS C:\Users\difao\.codyssey> Get-Content "$lab\파일.txt"
Get-Content : 'C:\Users\difao\.codyssey\권한실습\파일.txt' 경로에 대한 액세스가 거부되었습니다.
위치 줄:1 문자:1
+ Get-Content "$lab\파일.txt"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\Users\difao\.codyssey\권한실습\파일.txt:String) [Get-Content], Unau
thori    zedAccessException
    + FullyQualifiedErrorId : GetContentReaderUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetContentCommand
PS C:\Users\difao\.codyssey> Add-Content "$lab\파일.txt" "쓰기 시험" -ErrorAction Continue
Add-Content : 'C:\Users\difao\.codyssey\권한실습\파일.txt' 경로에 대한 액세스가 거부되었습니다.
위치 줄:1 문자:1
+ Add-Content "$lab\파일.txt" "쓰기 시험" -ErrorAction Continue
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\Users\difao\.codyssey\권한실습\파일.txt:String) [Add-Content], Unau
thori    zedAccessException
    + FullyQualifiedErrorId : GetContentWriterUnauthorizedAccessError,Microsoft.PowerShell.Commands.AddContentCommand
PS C:\Users\difao\.codyssey> icacls "$lab\폴더"
.\권한실습\폴더 NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
          BUILTIN\Administrators:(I)(OI)(CI)(F)
          BOOK-P2FAS03CTM\difao:(I)(OI)(CI)(F)
PS C:\Users\difao\.codyssey> icacls "$lab\폴더" /deny "${me}:(WD)"
처리된 파일: .\권한실습\폴더
PS C:\Users\difao\.codyssey> icacls "$lab\폴더"
.\권한실습\폴더 BOOK-P2FAS03CTM\difao:(DENY)(WD)
          NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
          BUILTIN\Administrators:(I)(OI)(CI)(F)
          BOOK-P2FAS03CTM\difao:(I)(OI)(CI)(F)
PS C:\Users\difao\.codyssey> ni "$lab\폴더\새파일.txt" -ItemType File -ErrorAction Continue
ni : 'C:\Users\difao\.codyssey\권한실습\폴더\새파일.txt' 경로에 대한 액세스가 거부되었습니다.
위치 줄:1 문자:1
+ ni "$lab\폴더\새파일.txt" -ItemType File -ErrorAction Continue
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\Users\difao\.codyssey\권한실습\폴더\새파일.txt:String) [New-Item],
Unauthor    izedAccessException
    + FullyQualifiedErrorId : NewItemUnauthorizedAccessError,Microsoft.PowerShell.Commands.NewItemCommand
PS C:\Users\difao\.codyssey> icacls "$lab\파일.txt" /remove:d $me
처리된 파일: .\권한실습\파일.txt
PS C:\Users\difao\.codyssey> icacls "$lab\폴더" /remove:d $me
처리된 파일: .\권한실습\폴더
PS C:\Users\difao\.codyssey> Add-Content "$lab\파일.txt" "복구 후 쓰기 시험"
PS C:\Users\difao\.codyssey> ni "$lab\폴더\복구후.txt" -ItemType File
디렉터리: C:\Users\difao\.codyssey\권한실습\폴더
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----      2026-10-30  오후 12:05              0 복구후.txt



Docker 설치 및 기본 점검
docker 버전 확인
PS C:\Users\difao\.codyssey> docker --version
Docker version 29.9.2, build a82e528
docker info
PS C:\Users\difao\.codyssey> docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Users\difao\.docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.6.1
    Path:     C:\Users\difao\.docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.97.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.9
    Path:     C:\Users\difao\.docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Users\difao\.docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Users\difao\.docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.2
    Path:     C:\Users\difao\.docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Users\difao\.docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.21
    Path:     C:\Users\difao\.docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Users\difao\.docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Users\difao\.docker\cli-plugins\docker-scout.exe

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 29.9.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: local
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 nvidia runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b7a25261e029d68978bbf290df0a62fc819b3c51
 runc version: v1.5.2-0-g29dd3dc2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.54.1-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 20
 Total Memory: 7.539GiB
 Name: docker-desktop
 ID: 535f3a16-ef05-414f-9ff2-f1c0401d0074
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8



4. Docker 기본 운영 명령 수행

PS C:\Users\difao> docker images    i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U
nginx:latest         600f7cd45e27        243MB         66.8MB
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
 
PS C:\Users\difao> docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED       STATUS                   PORTS     NAMES
9562d15c0d8e   hello-world   "/hello"   4 hours ago   Exited (0) 4 hours ago             vigorous_dewdney
 
docker ps의 경우 현재 실행 중인 컨테이너 목록을 보여주는 명령이다. 여기서 -a를 붙이면 중지된 컨테이너까지 확인할 수 있다.

PS C:\Users\difao> docker run -d --name ops-lab nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
b9b388a21ecf: Pull complete
12b85cff06fd: Pull complete
7cc330898115: Pull complete
7b229517ed22: Pull complete
2ec5fb117ffa: Pull complete
657023555a2a: Pull complete
26185d08a3ec: Pull complete
9b0f7dfe35b2: Pull complete
a4d5b1f57abc: Download complete
acc2d54c3743: Download complete
Digest: sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed0606
Status: Downloaded newer image for nginx:alpine
c3080411bec0e4dc8c3c01899cc8b044dcf2bc6d79edbc43668f4177a68431d4
PS C:\Users\difao> docker logs ops-lab   (docker logs -f ops-lab 의 경우에는 기존 로그를 보여준 뒤, 새 로그가 발생할 때마다 계속 출력한다.)
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/01 23:03:44 [notice] 1#1: using the "epoll" event method
2026/11/01 23:03:44 [notice] 1#1: nginx/1.31.8
2026/11/01 23:03:44 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/11/01 23:03:44 [notice] 1#1: OS: Linux 6.18.54.1-microsoft-standard-WSL2
2026/11/01 23:03:44 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/11/01 23:03:44 [notice] 1#1: start worker processes
2026/11/01 23:03:44 [notice] 1#1: start worker process 31
2026/11/01 23:03:44 [notice] 1#1: start worker process 32
2026/11/01 23:03:44 [notice] 1#1: start worker process 33
2026/11/01 23:03:44 [notice] 1#1: start worker process 34
2026/11/01 23:03:44 [notice] 1#1: start worker process 35
2026/11/01 23:03:44 [notice] 1#1: start worker process 36
2026/11/01 23:03:44 [notice] 1#1: start worker process 37
2026/11/01 23:03:44 [notice] 1#1: start worker process 38
2026/11/01 23:03:44 [notice] 1#1: start worker process 39
2026/11/01 23:03:44 [notice] 1#1: start worker process 40
2026/11/01 23:03:44 [notice] 1#1: start worker process 41
2026/11/01 23:03:44 [notice] 1#1: start worker process 42
2026/11/01 23:03:44 [notice] 1#1: start worker process 43
2026/11/01 23:03:44 [notice] 1#1: start worker process 44
2026/11/01 23:03:44 [notice] 1#1: start worker process 45
2026/11/01 23:03:44 [notice] 1#1: start worker process 46
2026/11/01 23:03:44 [notice] 1#1: start worker process 47
2026/11/01 23:03:44 [notice] 1#1: start worker process 48
2026/11/01 23:03:44 [notice] 1#1: start worker process 49
2026/11/01 23:03:44 [notice] 1#1: start worker process 50


PS C:\Users\difao> docker stats
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O        BLOCK I/O     PIDS
c3080411bec0   ops-lab   0.00%     17.05MiB / 7.539GiB   0.22%     1.7kB / 126B   0B / 12.3kB   21



5. 컨테이너 실행 실습

hello-world 실습

PS C:\Users\difao> docker run hello-world
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

ubuntu 실습  (ls, echo, cat /etc/os-release)

-it --interactive(컨테이너의 입력을 계속 열어둠.) + --tty(터미널처럼 사용할 수 있게 하다.)
PS C:\Users\difao> docker run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
f3f25b84a7d7: Pull complete
edafa32f5d9e: Pull complete
21594acc2d8b: Download complete
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
root@69c1855c341f:/#
root@69c1855c341f:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@69c1855c341f:/# echo "Hello from Ubuntu"
Hello from Ubuntu
root@69c1855c341f:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo



PS C:\Users\difao> docker run -dit --name aelab ubuntu bash
4c1173f98672ba6c9528975976f28794ca2108dc77f627c84edc00add7968e81
PS C:\Users\difao>
PS C:\Users\difao>
PS C:\Users\difao> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
4c1173f98672   ubuntu    "bash"    2 seconds ago   Up 2 seconds             aelab
PS C:\Users\difao>
PS C:\Users\difao>
PS C:\Users\difao> docker attach aelab
root@4c1173f98672:/#
root@4c1173f98672:/#
root@4c1173f98672:/# echo $$
1
root@4c1173f98672:/# read escape sequence
PS C:\Users\difao> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS              PORTS     NAMES
4c1173f98672   ubuntu    "bash"    About a minute ago   Up About a minute             aelab
PS C:\Users\difao>
PS C:\Users\difao>
PS C:\Users\difao> docker exec -it aelab bash
root@4c1173f98672:/#
root@4c1173f98672:/#
root@4c1173f98672:/# echo $$
8
root@4c1173f98672:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@4c1173f98672:/#
root@4c1173f98672:/# exit
exit
What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug aelab
    Learn more at https://docs.docker.com/go/debug-cli/
PS C:\Users\difao>
PS C:\Users\difao> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
4c1173f98672   ubuntu    "bash"    2 minutes ago   Up 2 minutes             aelab
PS C:\Users\difao> docker attach aelab
root@4c1173f98672:/# echo $$
1
root@4c1173f98672:/# exit
exit
PS C:\Users\difao> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

attach의 경우 실행한 프로세스를 종료할 때 컨테이너도 같이 종료된다. 그렇기에 사용을 종료하기 위해선 Ctrl+p, Ctrl+q를 활용하여 연결을 해제한다.
exec의 경우 새로 생성된 셸만 종료하기 때문에 컨테이너는 종료되지 않는다.


6. 기존 Dockerfile 기반 커스텀 이미지 제작.

여기서 이미지란? -> 프로그램을 실행하는 데 필요한 파일, 라이브러리, 설정 등을 포함하는 실행용 묶음이다. 이미지를 사용하면 프로그램과 필요한 구성 요소를 함께 관리할 수 있어, 실행 환경을 준비하고 배포하기 편리하다.
