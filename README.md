# docker-compose-study


## 설정
0. 기본 코드 설정
1. db 헬스체크 -> docker compose.yml에 설정
    db헬스체크 결과(위치=docker inspect <컨테이너 이름> 했을 때 state/Health에 있음)

    ```bash
    "Health": {
                "Status": "healthy",
                "FailingStreak": 0,
                "Log": [
                    {
                        "Start": "2026-03-30T11:25:48.936875082+09:00",
                        "End": "2026-03-30T11:25:49.005565938+09:00",
                        "ExitCode": 0,
                        "Output": "mysqladmin: [Warning] Using a password on the command line interface can be insecure.\nmysqld is alive\n"
                    },
                    {
                        "Start": "2026-03-30T11:25:59.018621306+09:00",
                        "End": "2026-03-30T11:25:59.099086432+09:00",
                        "ExitCode": 0,
                        "Output": "mysqladmin: [Warning] Using a password on the command line interface can be insecure.\nmysqld is alive\n"
                    },
                    {
                        "Start": "2026-03-30T11:26:09.110522813+09:00",
                        "End": "2026-03-30T11:26:09.182751283+09:00",
                        "ExitCode": 0,
                        "Output": "mysqladmin: [Warning] Using a password on the command line interface can be insecure.\nmysqld is alive\n"
                    },
                    {
                        "Start": "2026-03-30T11:26:19.185267911+09:00",
                        "End": "2026-03-30T11:26:19.238934473+09:00",
                        "ExitCode": 0,
                        "Output": "mysqladmin: [Warning] Using a password on the command line interface can be insecure.\nmysqld is alive\n"
                    },
                    {
                        "Start": "2026-03-30T11:26:29.243882331+09:00",
                        "End": "2026-03-30T11:26:29.311672961+09:00",
                        "ExitCode": 0,
                        "Output": "mysqladmin: [Warning] Using a password on the command line interface can be insecure.\nmysqld is alive\n"
                    }
                ]
            }
        },
    ```
2. app 헬스체크 -> Dockerfile에 설정
    app 헬스체크 결과(위치는 동일함)
## 실험
1. db가 떠야 app이 뜸
2. db가 항상 실패하도록 했을 때 
    app이 뜨지 않음

3. db를 중간에 중단하면?
- app컨테이너 자체는 죽지 않고 살아있음
- 하지만 app에 api를 날려본다면?
    - 500에러가 뜸  

4. db를 stop한 후에 보면 app이 unhealthy인데 다시 db를 up하면 app이 healthy로 돌아옴

## 트러블슈팅
1. spring boot의 application properties에 하드코딩 되어있어서 app이 뜨지 않음 -> 환경변수로 바꾼 후 다ㅣㅅ jar파일로 재빌드
2. 볼륨을 지우지 않아서 login할 때 에러가 뜸 -> docker compose down -v 명령어로 볼륨까지 지움