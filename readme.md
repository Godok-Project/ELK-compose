# 실행 전 설정법

- 본인이 사용할 비밀번호 입력하기 (비밀번호 설정은 실행 후에 함)
    - 변경해야할 파일
    - kibana.yml, logstash.conf, logstash.yml
    - 097531이라고 적힌 곳에 본인이 사용할 비밀번호 입력
    - username은 elastic이 기본값

# Compose 사용 후 설정법

- 현재 폴더에서 `docker-compose up`으로 docker-compose.yml 실행 
- ELK가 전부 돌아가는데 logstash에서 오류 발생 (비밀번호를 지정해주지 않았기 때문)
    - `docker exec -it {elasticsaerch 컨테이너 이름} /bin/bash`로 컨테이너 입장
    - `cd bin` -> `elasticsearch-setup-passwords interactive`로 비밀번호 설정
        - 한 8개정도 비밀번호를 설정하는데 전부 실행 전에 입력한 비밀번호로 설정하기
    - 그 후 `exit`으로 컨테이너 나오기
- mysql과 연결을 위해서 mysql_connector를 넣어줘야함 
    - 현재 폴더로 이동 
    - `docker cp mysql-connector-j-8.0.28.jar {logstash컨테이너 이름}:/usr/share/logstash/`로 connector를 logstash로 복사
        - RDS는 8.0.28이므로 위와 같이 작성하고 만약 로컬 mysql을 사용한다면 그에 맞는 버전을 사용하면됨
        - 저는 최신 버전이라 8.0.32 connector도 넣어놨습니다
- 엘라스틱 컨테이너 재시작

# 주의사항 

- 위와같이 설정하고 재시작하고 logstash도 재시작하면 logstash가 실행된다. 
- 그러면 logstash.conf 파일이 바로 실행되므로 일단 sql문을 주석처리 해놓았음 
- 그리고 현재 logstash.conf에 연결된 DB는 RDS
- DB랑 sql문 원하는대로 설정하고 재시작하면 진행될겁니다.