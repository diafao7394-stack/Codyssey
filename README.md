<img width="2768" height="240" alt="image" src="https://github.com/user-attachments/assets/b0869397-e7de-4f6d-8139-249d49c61d29" />
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

docker run -dit --name ops-lab nginx:alpine
-dit
--detach 컨테이너를 백그라운드에서 실행하고 PowerShell 프롬프트로 돌아온다.
--interactive 터미널이 연결되어 있지 않아도 컨테이너의 표준 입력을 열어둔다.
--tty 컨테이너에 가상 터미널을 할당한다.
  
<img width="2080" height="1372" alt="image" src="https://github.com/user-attachments/assets/5d21dd4f-afa2-4663-9875-23f273b442b5" />  

아래는 docker stats를 입력하였을 때 출력되는 화면이다. Ctrl+c를 눌러서 해당 화면에서 탈출할 수 있다.  

<img width="2066" height="262" alt="image" src="https://github.com/user-attachments/assets/8cb58aaa-c4cd-42f1-bd60-011e4c9011ae" />


PS C:\Users\difao> docker stats
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O        BLOCK I/O     PIDS
c3080411bec0   ops-lab   0.00%     17.05MiB / 7.539GiB   0.22%     1.7kB / 126B   0B / 12.3kB   21

<img width="2406" height="850" alt="image" src="https://github.com/user-attachments/assets/a6e4b5d6-a31d-4ff4-9135-657f7a8ad153" />  
docker 에서 실행중인 컨테이너와 종료된 컨테이너들을 중단하고, 삭제하는 명령어들이다.
docker stop XXXXX 를 입력하면 name 이 XXXXX에 해당하는 컨테이너를 중지시키고,
docker rm XXXXX 를 입력하면 중지된 상태인, name 이 XXXXX인 컨테이너를 삭제한다.

## 5. 컨테이너 실행 실습

hello-world 실습  

<img width="1550" height="864" alt="image" src="https://github.com/user-attachments/assets/beaa2c25-fe6a-4dcc-ab00-0381c8825445" />



ubuntu 실습  (ls, echo, cat /etc/os-release)  

<img width="2038" height="1312" alt="image" src="https://github.com/user-attachments/assets/38e63e15-1995-4400-91ed-b3e9dbcc6e48" />  

Ubuntu bash 종료 직전 Ctrl+c를 입력하였기 때문에 130이 출력되는 것을 확인할 수 있다.  

<img width="2212" height="690" alt="image" src="https://github.com/user-attachments/assets/7c533e06-a1bc-4013-adf8-d99e4d049a12" />  

attach 방식으로 로그인 후 강제탈출 (Ctrl+p, Ctrl+q)
탈출 이후 docker ps 로 실행 중인 컨테이너를 확인했을 때 --name aelab 으로 출력되는 ubuntu bash가 아직 존재함을 파악할 수 있음.
echo $$ 값은 1로 출력.  

<img width="2142" height="664" alt="image" src="https://github.com/user-attachments/assets/eaa2978b-e00a-4538-87dd-cc93f72fb6ed" />
ubuntu bash를 exec 방식으로 접속.
exit 방식을 사용하여 종료하였음에도 docker ps로 실행 중인 컨테이너를 확인했을 때 ubuntu bash가 아직 존재하는 걸 확인할 수 있음.
echo $$ 값은 8로 출력.  

<img width="2094" height="378" alt="image" src="https://github.com/user-attachments/assets/870b6332-c5be-4af4-a9d9-f99d83df1ab8" />  
이번에도 echo $$ 값은 위 attach 방식으로 진행한 것과 같이 1로 출력.
하지만 exec 방식과는 다르게 exit로 종료를 했을 때 차이점이 발생한다.
docker ps를 입력해보면 exec 방식으로 접속을 하였을 땐 bash가 유지되었으나 attach 는 exit로 종료하였을 때 실행중인 컨테이너 목록에서 제외된 것을 확인할 수 있다.  

attach의 경우 실행한 프로세스를 종료할 때 컨테이너도 같이 종료된다. 그렇기에 사용을 종료하기 위해선 Ctrl+p, Ctrl+q를 활용하여 연결을 해제한다.
exec의 경우 새로 생성된 셸만 종료하기 때문에 컨테이너는 종료되지 않는다.


## 6. 기존 Dockerfile 기반 커스텀 이미지 제작.

여기서 이미지란? -> 프로그램을 실행하는 데 필요한 파일, 라이브러리, 설정 등을 포함하는 실행용 묶음이다. 이미지를 사용하면 프로그램과 필요한 구성 요소를 함께 관리할 수 있어, 실행 환경을 준비하고 배포하기 편리하다.

dockerfile이란? -> 이미지를 어떻게 만들지 적어둔 텍스트 파일이다. 보통 확장자 없이 dockerfile이며 docker build를 실행했을 때 docker가 해당 파일의 지시를 수행하여 docker image를 만든다.
확장자가 없기에 visualstudioCode를 사용하여 dockerfile 파일을 만드는 것이 일반적이다.

<img width="958" height="162" alt="image" src="https://github.com/user-attachments/assets/cb35c9f7-e6e3-4f70-bdd7-7bf5d0491815" />  
dockerfile에 대한 정보이다.
이는 index.html을 기본페이지로 제공하는 nginx 이미지를 만들 수 있다.
FROM nginx:1.30.6-alpine 은 새 이미지의 출발점으로 지정하는 코드이다.
Copy는 빌드에 사용하는 폴더의 index.html에 접근하여 이미지 내부의 usr/share/ngninx/html/index.html에 복사한다.
해당 이미지로 컨테이너를 실행하면 nginx 가 복사된 페이지를 제공한다.

<img width="2836" height="194" alt="image" src="https://github.com/user-attachments/assets/a6aa167d-432e-42c5-8a7e-51960f8fc391" />  

<img width="2768" height="240" alt="image" src="https://github.com/user-attachments/assets/66f90c02-7f3f-47e6-9c6e-b443a9c5303e" />  

<img width="1120" height="458" alt="image" src="https://github.com/user-attachments/assets/19ab0cf1-8e73-4e12-bae2-8502bfba00c6" />  

dockerfile 기반으로 커스텀 이미지를 제작하여 포트매핑을 한 것을 확인할 수 있다.


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
