### 설치 : Ubuntu(UTM/WSL2)
1. /usr/local 디렉토리에 대한 권한 변경
    - `sudo chown -R ${USER}:${USER} /usr/local`
        - `${USER}` : 현재 로그인한 사용자 이름
2. Flink 1.20 다운로드 및 압축 해제
    1. `sudo wget https://archive.apache.org/dist/flink/flink-1.20.0/flink-1.20.0-bin-scala_2.12.tgz`
    2. `sudo tar -xvzf flink-1.20.0-bin-scala_2.12.tgz`
3.  Flink 설정 변경
    1. `vi flink-1.20.0/conf/flink-conf.yaml`
        1. 다음 항목들을 추가
            - `jobmanager.bind-host: 0.0.0.0`
            - `rest.address: 0.0.0.0`
            - `rest.bind-address: 0.0.0.0`
            - `jobmanager.rpc.address: localhost`


### 실행
1. 클러스터 시작 및 UI 접속
    1. `flink-1.20.0/bin/start-cluster.sh`
    2. 웹 UI 접속: `http://localhost:8081`
    3. Task Slot 개수 잘 뜨는지 확인
2. Flink 클러스터 종료
    1. ``flink-1.20.0/bin/stop-cluster.sh`

### 테스트 실행
1. 터미널 1에서 8000번 포트를 열어서 텍스트를 받을 준비
    - `nc -l 8000`
2. 터미널 2에서 예제 실행
    - `flink-1.20.0/bin/flink run flink-1.20.0/examples/streaming/SocketWindowWordCount.jar --hostname localhost --port 8000`
3. 웹 UI에서 TaskManager Stdout 실시간 확인
    - 또는 터미널에서 확인
        - `cd flink-1.20.0/`
        - `tail -f log/flink-*.out`
        - `tail`: 파일 마지막 부분 출력
        - `-f`: 파일 변경사항을 실시간으로 추적하며 출력
        - `log/flink-*.out`: Flink 로그 파일 전체 지정 (`standalonesession`, `taskexecutor` 등 포함)