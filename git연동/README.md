\# 10. Git 설정 및 GitHub 연동



\## 실습 목적

Git 사용자 정보와 기본 브랜치를 설정하고,

GitHub 저장소에 실습 자료를 업로드하여 연동을 확인한다.



\## 1. 실행 환경 및 Git 설정



\- Git 버전: 2.45.2.windows.1

\- Git 작성자 이름: gonyeon

\- Git 작성자 이메일: difaolo@naver.com

\- 기본 브랜치 설정: main

\- 현재 작업 브랜치: main



다음 명령으로 설정을 확인하였다.



```powershell

git --version

git config user.name

git config user.email

git config --get init.defaultBranch

git branch --show-current

```



\[git config --list 전체 실행 결과](./git-config.txt)



\## 2. GitHub 저장소 연결



\- 저장소: https://github.com/diafao7394-stack/Codyssey

\- 원격 저장소 이름: origin



다음 명령으로 저장소를 clone하고 원격 주소를 확인하였다.



```powershell

git clone "https://github.com/diafao7394-stack/Codyssey.git"

Set-Location ".\\Codyssey"

git remote -v

```



\## 3. 실습 자료 업로드



다음 명령으로 실습 자료를 커밋하고 업로드한다.



```powershell

git add -- "git연동"

git commit -m "docs: add Git integration practice"

git push

```



\## 4. 검증 결과

실제 push 성공 후 결과와 증거 화면을 추가한다.

