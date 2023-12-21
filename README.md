# CI / CD
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
