# CI / CD
- 과정
- ubuntu 생성 후 업데이트 업그레이드

```
sudo apt update

sudo apt upgrade


```

- 계정 비밀번호 설정
```
ssh -i ~/.ssh/mykey [사용자이름]@[서버 IP]

SSH 설정 파일 열기: 먼저, SSH 서버의 설정 파일인 sshd_config를 열어야 합니다. 이를 위해 다음 명령어를 사용합니다:

sudo vim /etc/ssh/sshd_config

vim 대신 nano나 다른 텍스트 편집기를 사용할 수도 있습니다.

비밀번호 인증 활성화: sshd_config 파일에서 PasswordAuthentication 설정을 찾아서 yes로 변경합니다. 만약 이 줄이 주석 처리되어 있다면 (즉, 줄 앞에 #가 있다면), 주석 처리를 제거하고 yes로 설정합니다.

PasswordAuthentication yes

파일 저장 및 닫기: 변경 사항을 저장하고 편집기를 종료합니다. vim을 사용하는 경우, :wq를 입력하고 엔터를 누릅니다.

SSH 서비스 재시작: 설정 변경 사항을 적용하기 위해 SSH 서비스를 재시작합니다.

sudo systemctl restart sshd

사용자 계정 비밀번호 설정: 서버에 이미 계정이 설정되어 있다면, 각 사용자 계정에 대해 비밀번호를 설정할 수 있습니다. 다음 명령어를 사용하여 비밀번호를 설정합니다:

sudo passwd [사용자이름]

여기서 [사용자이름]은 비밀번호를 설정하려는 계정의 이름입니다.

이렇게 하면 원격으로 SSH를 통해 서버에 접속할 때 비밀번호 인증을 요구하게 됩니다. 비밀번호는 강력하고 예측하기 어려운 것으로 설정하는 것이 좋습니다. 또한, SSH 키 기반 인증과 비밀번호 기반 인증을 함께 사용하면 보안을 더욱 강화할 수 있습니다.

```

- Git 연결 및 Pull

```

mkdir 레포명

cd 레포명

git init

git remote add origin [원격-리포지토리-URL]

git pull origin main

git config --global credential.helper store

sudo apt install python3-pip

sudo apt install python3.10-venv

python3 -m venv venv

source venv/bin/activate
```

- Gunicorn 세팅 및 실행

```
pip install gunicorn # 구니콘 설치

cd /home/{사용자명}/{프로젝트 루트 디렉터리}

gunicorn --bind 0:5000 {프로젝트이름}.wsgi:application
"""
5000번 포트로 WSGI 서버(gunicorn)와 연결된 WSGI 애플리케이션(django)를 실행시킨다.
# nginx

[2023-11-28 10:56:09 +0900] [52669] [INFO] Starting gunicorn 21.2.0
[2023-11-28 10:56:09 +0900] [52669] [INFO] Listening at: http://0.0.0.0:5000 (52669)
[2023-11-28 10:56:09 +0900] [52669] [INFO] Using worker: sync
[2023-11-28 10:56:09 +0900] [52670] [INFO] Booting worker with pid: 52670

이렇게 나오면 WSGI 서버와 애플리케이션이 실행된것입니다.
"""

```
포트번호 5000번 열면 됩니다.
![image](https://github.com/maxkim77/CI/assets/141907655/87d4744f-2599-424f-a4fb-2ff60db62ace)

- 유닉스(우분투) 소켓을 활용한 수행항법
유닉스 소켓: IPC 소켓이라고도 불리며 시스템내  프로세스 간의 데이터를 교환하기 위한 수단.

```
cd /home/{사용자명}/{프로젝트 루트 디렉터리}

gunicorn --bind unix:/tmp/gunicorn.sock {프로젝트명}.wsgi:application

"""
[2023-11-28 11:56:09 +0900] [52669] [INFO] Starting gunicorn 21.2.0
[2023-11-28 11:56:09 +0900] [52669] [INFO] Listening at: unix:/tmp/gunicorn.sock (52669)
[2023-11-28 11:56:09 +0900] [52669] [INFO] Using worker: sync
[2023-11-28 11:56:09 +0900] [52670] [INFO] Booting worker with pid: 52670
"""

```

- 우분투에 Gunicron 서비스 등록
```
sudo vim /etc/systemd/system/{프로젝트명}.service # 편집기 실행
### {프로젝트명}.service

[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu # 유저이름 확인해주세요.
Group=ubuntu
WorkingDirectory=/home/ubuntu/{프로젝트 디렉토리} # 프로젝트 루트 디렉터리
EnvironmentFile=/home/ubuntu/{프로젝트 디렉토리}/.env # 환경변수 파일
ExecStart=/home/ubuntu/{프로젝트 디렉토리}/venv/bin/gunicorn \ #가상환경에 설치된 gunicorn
        --workers 2 \ #워커 갯수
        --bind unix:/tmp/gunicorn.sock \ # WSGI실행 명령
        {프로젝트명}.wsgi:application # WSGI(Django) 애플리케이션

[Install]
WantedBy=multi-user.target


# 서비스 실행
sudo systemctl start {위에서 작성했던 파일명}

# 실행확인
sudo systemctl status {위에서 작성했던 파일명} # .service 빼고입니다.

sudo systemctl enable {위에서 작성했던 파일명} # .service 빼고입니다.
# 서버가 재실행 될 때 Gunicorn 서비스가 자동 실행되게 만들어 줍니다.

sudo systemctl restart {위에서 작성했던 파일명} # .service 빼고입니다.
# 서비스 재실행 명령
```

- 4. Nginx 설정
 
```
sudo vim /etc/nginx/sites-enabled/default # nginx 설정파일 편집 실행

server{
	root /var/www/html;
	server_name # 도메인 주소 없으면 IPV4 주소

	location /api/v1/{ # 프론트에서 오는 /api/v1/ 으로 시작되는 모든 요청 핸들링
             include proxy_params;
             proxy_pass http://unix:/tmp/gunicorn.sock;
        }

}
```




- 레포지토리 설정 → Secrets and variables 클릭

- Git action에서 사용할 시크릿 키 추가하기

    - DEBUG
    - SECRET_KEY
    - SERVER_HOST
    - SERVER_PASSWORD
    - SERVER_USER

- 상단 Actions 탭 클릭(액션 탭이 없다면 아래처럼 퍼미션을 수정해야합니다.)
 ![image](https://github.com/maxkim77/CI/assets/141907655/2193bc01-4375-422d-8969-0deb177e91d2)

- set up a workflow yourself 클릭
![image](https://github.com/maxkim77/CI/assets/141907655/b71a123d-82fe-4430-9852-c9200f072ce7)

- 깃허브 액션 트리거 파일 생성화면
![image](https://github.com/maxkim77/CI/assets/141907655/29708a7b-59b0-4708-8d0b-644bd6caac04)

- yaml 파일 생성
```
name: Django CI/CD

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 체크아웃 레포지토리
        uses: actions/checkout@v3

      - name: 파이썬 설정
        uses: actions/setup-python@v3
        with:
          python-version: '3.11'

      - name: 의존성 설치
        run: |
          pip install --upgrade pip
          pip install -r requirements.txt
          
      - name: 서버 배포
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }} 
          username: ${{ secrets.SERVER_USER }}
          password: ${{ secrets.SERVER_PASSWORD }}
          script: |
						set -e
            cd /home/ubuntu/{레포지토리주소}
            git pull origin main

            echo "SECRET_KEY=\"${{ secrets.SECRET_KEY }}\"" > .env
            echo "DEBUG='${{ secrets.DEBUG }}'" >> .env

            source venv/bin/activate
            pip install -r requirements.txt
            sudo systemctl restart {아까등록한서비스이름}.service
```
오류상황:

- uwsgi 설정 문제와 Django 모듈 누락 이슈.


해결책:
- uwsgi.ini 파일에 http = :8000 추가.


오류상황:
- uwsgi가 이미 실행 중이라는 오류(Address already in use).


해결책:
- lsof -i :8000 및 lsof -i :8080 사용하여 포트 사용 상태 확인 후 필요시 해당 프로세스 종료(kill).


오류상황:

![image](https://github.com/maxkim77/CI/assets/141907655/ba9ef99c-1c9d-4daf-af30-67fb18c34fa0)

- git 저장소 연결 및 경로 설정 문제.


해결책:


- git remote 및 가상환경 위치 재설정


오류상황:


- ModuleNotFoundError: No module named 'django' 오류.

  
해결책:


- 가상환경 확인 및 필요한 패키지 재설치(pip install -r requirements.txt).
