
# 프로젝트 개요
1) 실행환경
- OS: Windows 11 
- Shell: PowerShell 
3- Docker: 29.9.2
- Git: 2.45.2.windows.1


2) 수행 체크리스트

-[O] 터미널 조작 로그 기록

-[O] 권한 실습 및 증거 기록

-[O] Docker 설치 및 기본 점검

-[O] Docker 기본 운영 명령 수행

-[O] 컨테이너 실행 실습

-[O] 기존 Dockerfile 기반 커스텀 이미지 제작

-[O] 포트 매핑 및 접속 증거

-[O] Docker 볼륨 영속성 검증

-[O] Git 설정 및 GitHub 연동

3) 프로젝트 디렉토리 구조

Codyssey - README.md  -프로젝트를 진행한 내용에 대하여 간략하게 정리.
         - 입출력.md  - 터미널에 입력한 기록과 그에 대한 출력값을 고스란히 기록.
         - git 연동   - Git 설정과 연동 증거에 대한 자료.


# 프로젝트 진행 사항

## 1. 터미널 조작 로그 기록 (윈도우즈 파워셸)

#### 현재 위치 확인, 현재 위치 이동하기

pwd
cd ~~

<img width="598" height="794" alt="image" src="https://github.com/user-attachments/assets/3ad33fb4-ff0c-4baa-b4a0-6cdc1ecf7658" />

#### 빈 폴더 생성, 빈 파일 생성

ni test.txt
mkdir 1주차

<img width="1158" height="804" alt="image" src="https://github.com/user-attachments/assets/ad6b605c-f1b3-4891-b96e-30f228c6e95e" />
<img width="1160" height="1232" alt="image" src="https://github.com/user-attachments/assets/df8d201b-2ca0-4036-9a18-d02ddf9edf76" />

#### 폴더 내 파일 목록 확인(숨김 파일 포함)

ls -Force

<img width="1160" height="364" alt="image" src="https://github.com/user-attachments/assets/eec333c0-1f4d-428d-a9ee-9e6ae55fbfc6" />

#### 파일 복사,  파일 이동

cp test.txt c.txt
mv c.txt a

<img width="1174" height="1046" alt="image" src="https://github.com/user-attachments/assets/87c667ba-8c37-422c-a652-49bca38da41f" />

#### 파일 이름 변경

ren (현재 파일 이름) (바꿀 파일 이름)
rm (삭제할 파일 이름)

<img width="656" height="222" alt="image" src="https://github.com/user-attachments/assets/1ce5a352-18e5-4f0d-aa61-4f8a033106bc" />

#### 파일 내 내용 변경, 파일 내용 확인

echo "~~~~" > test.txt   덮어쓰기
echo "~~~~" >> test.txt  붙여쓰기
cat test.txt             텍스트 읽기

<img width="878" height="472" alt="image" src="https://github.com/user-attachments/assets/4be3232f-de2c-4cf7-ae86-51f8fc6e3820" />


#### 프롬프트 메시지 출력값 변경하기

PS C:\Users\difao\.codyssey\docker-custom-lab> function prompt { "> " }

<img width="994" height="236" alt="image" src="https://github.com/user-attachments/assets/9730162f-2f5a-4b44-9ab2-3bd217a89a57" />


## 2. 권한 실습 및 증거 기록.
권한을 확인/변경하는 명령을 수행하고 변경 전/후 비교를 기술 문서에 남긴다.    
최소 요구 : 파일 1개, 디렉토리 1개에 대해 권한 변경 실험을 수행한다.
icacls는 파일이나 폴더의 접근 권한(ACL)을 확인하거나 변경하는 명령어이다.
icacls = Integrity Control Access Control List    


<img width="1122" height="1012" alt="image" src="https://github.com/user-attachments/assets/2c6c2b07-d09a-4a25-9599-206d39d5d37b" />

I => Inherited (상속됨)  
F => Full control(모든 권한)


<img width="1026" height="428" alt="image" src="https://github.com/user-attachments/assets/9eeecbda-2ede-4c21-91b6-cc8beb526580" />

/deny => 거부  
/deny "${me}:w" (사용자에 대한 쓰기 명령 거부)

위에서 이미 $me 에 대한 값을 whoami 로 설정했었다.  
whoami 는 현재 명령을 실행하는 사용자의 게정에 대한 값을 알려준다.

<img width="2308" height="336" alt="image" src="https://github.com/user-attachments/assets/ccdc1d63-1785-45ad-af3f-c96e3aababec" />

해당 이미지에서 Get_Content를 하였더니 액세스가 거부되었다.  
이는 읽기 권한도 거부당한 것으로 해석이 된다.
/deny 로 읽기 권한과 쓰기 권한을 거부한다면, 남은 권한에도 필요한 공통 권한인 SYNCHRONIZE 까지 거부되기 때문이다.  

<img width="1062" height="566" alt="image" src="https://github.com/user-attachments/assets/dca90cf0-f934-40b7-a717-65c5e26c985f" />
<img width="2300" height="330" alt="image" src="https://github.com/user-attachments/assets/afdf9aad-3f25-486c-b8ad-5ea857b022e4" />
  
"$lab\폴더" 에 me 에 대한 WD의 권한을 거절했다.  
WD는 Write data, add file등에 대한 권한이기에 ni를 활용한 새로운 파일 생성에 요청이 거절당하는걸 확인할 수 있다.  

<img width="1288" height="1162" alt="image" src="https://github.com/user-attachments/assets/f979ed2b-d573-4ced-bab0-0308b1268bc9" />  

기존에 icacls로 쓰기 권한이 거절된 파일, 새로울 파일 생성이 거절된 폴더에 대한 권한을 삭제했다.
그 후 파일.txt에 새로운 내용을 기재한 후 내용변경을 확인했다.
폴더 안에 새로운 파일을 생성하여 폴더 안의 목록들을 확인하였다.  

해당 실습은 파워셸에서 진행하였기에 icacls 명령어를 사용하였다.
우분투 환경에서도 권한에 대한 실습을 진행할 수 있는데, chmod 라는 명령어를 사용할 수 있다.  

chmod xxx test.txt
권한명령어 -> rwx비트의 숫자 표기법 -> 적용할 파일  

rwx비트의 숫자 표기법은 다음과 같다.
r(Read) = 4
w(Write) = 2
x(Execute) = 1  

비트의 배열은 다음과 같다.
(소유자)(그룹)(그 외 사용자)
700의 경우 소유자에게 Read(4)+Write(2)+Execute(1) 모든 권한을 가지고 있는 것을 확인할 수 있다.
600의 경우 소유자에게 Read(4)+Write(2) 권한만 가지고 있는 것을 확인할 수 있다.

## 3. Docker 설치 및 기본 점검  

<img width="1490" height="1252" alt="image" src="https://github.com/user-attachments/assets/183903af-26f3-4f36-a5f0-bd4f3b6bb255" />

docker --version 을 입력하였을 때 출력 값은 다음과 같다. Docker version 29.9.2, build a82e528  
여기에서 29.9.2가 Docker CLI 버전이다. 또한 build 뒤에 있는 a82e528은 빌드 식별자이다.

docker info에서 확인할 수 있는 정보는 다음과 같다.
Context:    desktop-linux   docker의 현재 상태를 확인할 수 있고 플러그 인의 출력이 종료된 후 나오는 Server 출력의 상단부에서 다음과 같은 출력을 확인할 수 있다.  

  Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 5
 Server Version: 29.9.2  

 현재 컨테이너는 1개이며 > Running - 실행 중인 것은 0개, Paused - 정지 된 것은 0개, Stopped - 중지 된 것은 1개로 출력된 것을 확인할 수 있다.
 현재 보유한 이미지의 개수가 5개인 것을 확인할 수 있다.
 현재 Server version이 29.9.2로 출력되는 것을 확인할 수 있는데 위의 문단에서 docker의 CLI 버전이 29.9.2로 docker의 CLI 버전과 같다. Server의 출력이 정상적으로 이루어졌으므로, CLI가 Docker Engine에 연결해 정보를 조회했음을 알 수 있다.

 <img width="1270" height="1288" alt="image" src="https://github.com/user-attachments/assets/fae5de0a-74f3-4089-bc6b-a40f85bb76aa" />  

docker desktop status를 입력했을 땐 stopped가 표시되엇다. 이어서 docker --context desktop-linux version을 입력하였을 때 Server 항목에 Linux Docker Engine 버전 29.9.2가 출력되면서 해당 명령 실행 시점에 데몬이 응답했음을 확인했다.

## 4. Docker 기본 운영 명령 수행

docker images
docker ps -a

docker images로 이미지 목록을 확인하고, docker ps -a 로 종료된 컨테이너를 확인하였다. -a 는 종료된 컨테이너를 포함한 모든 컨테이너를 출력하기 때문에 -a 를 제외하고 입력한다면 종료된 컨테이너는 출력하지 않는다.



<img width="2870" height="470" alt="image" src="https://github.com/user-attachments/assets/18aa7e5a-6f8f-4cba-9a81-707c9e1aabad" />

PS C:\Users\difao> docker images    i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U
nginx:latest         600f7cd45e27        243MB         66.8MBㅑ
  ::1/128
 Live Restore Enabled: false
 Firewall Backend: iptables
 
PS C:\Users\difao> docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED       STATUS                   PORTS     NAMES
9562d15c0d8e   hello-world   "/hello"   4 hours ago   Exited (0) 4 hours ago             vigorous_dewdney
 
docker ps의 경우 현재 실행 중인 컨테이너 목록을 보여주는 명령이다. 여기서 -a를 붙이면 중지된 컨테이너까지 확인할 수 있다.

PS C:\Users\difao> docker run -dit --name ops-lab nginx:alpine
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



## 5. 컨테이너 실행 실습

hello-world 실습  

<img width="1550" height="864" alt="image" src="https://github.com/user-attachments/assets/beaa2c25-fe6a-4dcc-ab00-0381c8825445" />


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


## 6. 기존 Dockerfile 기반 커스텀 이미지 제작.

여기서 이미지란? -> 프로그램을 실행하는 데 필요한 파일, 라이브러리, 설정 등을 포함하는 실행용 묶음이다. 이미지를 사용하면 프로그램과 필요한 구성 요소를 함께 관리할 수 있어, 실행 환경을 준비하고 배포하기 편리하다.

PS C:\Users\difao\.codyssey\docker-custom-lab> docker image ls my-web
                                                                                                   i Info →   U  In Use
IMAGE   ID             DISK USAGE   CONTENT SIZE   EXTRA
PS C:\Users\difao\.codyssey\docker-custom-lab> docker build -t my-web:1.0 .
[+] Building 13.1s (8/8) FINISHED                                                                  docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 111B                                                                               0.0s
 => [internal] load metadata for docker.io/library/nginx:1.30.6-alpine                                             3.7s
 => [auth] library/nginx:pull token for registry-1.docker.io                                                       0.0s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load build context                                                                                  0.0s
 => => transferring context: 244B                                                                                  0.0s
 => [1/2] FROM docker.io/library/nginx:1.30.6-alpine@sha256:0e52bd054126cdc040b1276c0af347aeb02a41b9cfe305d609476  8.3s
 => => resolve docker.io/library/nginx:1.30.6-alpine@sha256:0e52bd054126cdc040b1276c0af347aeb02a41b9cfe305d609476  0.0s
 => => extracting sha256:7b229517ed222597277036d94892173207d01a731ab3a88eeb1de2e963a2c788                          0.1s
 => => sha256:5b8cb54cfa408030aaa43d7bffa9141e0ae6f8a479df13c7aa152100cca358fb 20.31MB / 20.31MB                   7.8s
 => => sha256:bb658a2541968d2b90a8ee165ad08b7f71055c7f7832bdff98e2de2d8b90cf14 1.40kB / 1.40kB                     0.7s
 => => sha256:81b64ae234ef95f3cfc19b7f6c45a511eccbefa6148889ee33ed43c6c3f39939 1.21kB / 1.21kB                     0.8s
 => => sha256:e125989d16f03d1e00b60afdd9abe46ed1437ed63b73926d036a5537f6be770a 403B / 403B                         0.8s
 => => sha256:885725a63a7cf1d4d5aac105944b35ece2b69a8c2cc6f7040f04727b7b8a63d2 964B / 964B                         0.3s
 => => sha256:f19a2d3b695178ba0e57d473535f74f5d934f45e191de8334e730f83d2c62704 627B / 627B                         0.4s
 => => sha256:9f0afb44f10e826253b342c0265e188067c2f0a30c782a52e329928a763adc60 1.88MB / 1.88MB                     1.3s
 => => extracting sha256:9f0afb44f10e826253b342c0265e188067c2f0a30c782a52e329928a763adc60                          0.1s
 => => extracting sha256:f19a2d3b695178ba0e57d473535f74f5d934f45e191de8334e730f83d2c62704                          0.0s
 => => extracting sha256:885725a63a7cf1d4d5aac105944b35ece2b69a8c2cc6f7040f04727b7b8a63d2                          0.0s
 => => extracting sha256:e125989d16f03d1e00b60afdd9abe46ed1437ed63b73926d036a5537f6be770a                          0.0s
 => => extracting sha256:81b64ae234ef95f3cfc19b7f6c45a511eccbefa6148889ee33ed43c6c3f39939                          0.0s
 => => extracting sha256:bb658a2541968d2b90a8ee165ad08b7f71055c7f7832bdff98e2de2d8b90cf14                          0.0s
 => => extracting sha256:5b8cb54cfa408030aaa43d7bffa9141e0ae6f8a479df13c7aa152100cca358fb                          0.4s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                         0.1s
 => exporting to image                                                                                             0.8s
 => => exporting layers                                                                                            0.1s
 => => exporting manifest sha256:b6185604fda7bd13b99884d62502017ce5e9b00a61774d297f594b366637f749                  0.0s
 => => exporting config sha256:ccb2e1e59ef940e68a650949a86d489ba4a29b6df428b6d0a96bdd62c8d0d0c3                    0.0s
 => => exporting attestation manifest sha256:a00dd29ecefa1b89cdbe366502c15244f255921f24a1c69da9b0cfc12f121c69      0.0s
 => => exporting manifest list sha256:1840870e263fe0fd37da960f369db934e0911a18766a96f986c4848ddc85b45a             0.0s
 => => naming to docker.io/library/my-web:1.0                                                                      0.0s
 => => unpacking to docker.io/library/my-web:1.0                                                                   0.6s
PS C:\Users\difao\.codyssey\docker-custom-lab> docker image ls my-web
                                                                                                   i Info →   U  In Use
IMAGE        ID             DISK USAGE   CONTENT SIZE   EXTRA
my-web:1.0   1840870e263f       92.7MB         26.1MB
PS C:\Users\difao\.codyssey\docker-custom-lab> docker run -d --name my-web-container -p 8080:80 my-web:1.0
7673664c7739381e7706f718f88f525f9c7a5d2cb73e895cd6433b38973a5d6a
PS C:\Users\difao\.codyssey\docker-custom-lab> docker ps --filter "name=my-web-container"
CONTAINER ID   IMAGE        COMMAND                   CREATED          STATUS          PORTS                                     NAMES
7673664c7739   my-web:1.0   "/docker-entrypoint.…"   37 seconds ago   Up 36 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-container

docker inspect 컨테이너 이름 입력 my web 컨테이너에 대한 자세한 정보들이 출력된다. 포트정보 포함 내용 존재 해당정보 첨부

## 7. Docker 포트 매핑 및 접속 증거.

포트 매핑이란 내 pc의 호스트의 포트와 컨테이너 내부의 포트와 연결하여 호스트를 통하여 컨테이너의 서비스에 접근할 수 있도록 하는 방식이다.
해당 실습에서는 -p 8080:80 옵션을 사용하여 8080번 포트를 컨테이너의 80번 포트에 연결하였다. 이 설정을 통하여 브라우저에서 컨테이너 내부의 Nginx 웹 서비스에 접근할 수 있다.

<img width="2318" height="528" alt="image" src="https://github.com/user-attachments/assets/0c40e393-ac14-4058-ac96-1423afeb252c" />

## 8. Docker 볼륨 영속성 검증

Docker에서 볼륨이란 외부저장소와 비슷한 개념을 가지고 있다. 보통 컨테이너를 삭제하게 될 경우 해당 컨테이너에 속한 모든 파일들이 같이 사라지게 된다. 볼륨은 이와 같은 상황에서 대처를 하기 위해 존재한다. 데이터를 볼륨에 저장하게 될 시, 컨테이너를 삭제하여도 해당 데이터를 유지할 수 있다.

해당 실습에서는 아래와 같은 순서로 실습을 진행하였다.

1. Docker 볼륨 생성 ==> my_web_data
2. volume-before 컨테이너 생성, 컨테이너의 /data 경로에 my-web-data 연결.  
3. docker inspect로 볼륨이 정상적으로 연결되었는지 확인했다.
4. /data/proof.txt 파일을 만들고 volume-persistence-test-001이라는 내용을 저장했다.
5. 파일을 읽어 저장한 내용이 출력되는지 확인했다.
6. volume-before 컨테이너를 중지하고 삭제했다.
7. 컨테이너는 삭제되었지만 my-web-data 볼륨은 남아 있는지 확인했다.
8. 새로운 volume-after 컨테이너를 만들고, 같은 my-web-data 볼륨을 /data에 연결했다.
9. 파일을 다시 생성하지 않고 읽었을 때 동일한 내용이 출력되는 것을 확인하여, 데이터 영속성을 검증했다.


<img width="2838" height="1596" alt="스크린샷 2026-11-05 115418" src="https://github.com/user-attachments/assets/4fc111f6-e572-40a8-92de-077126764cf1" />


9. Git 설정 및 GitHub 연동

https://github.com/diafao7394-stack/Codyssey/blob/main/git%EC%97%B0%EB%8F%99/git-config.txt
해당 위치에 Github에 연동하여 터미널로 커밋한 자료를 확인할 수 잇다.
