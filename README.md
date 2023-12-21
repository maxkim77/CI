# CI / CD
- 과정

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
