# Nginx 로깅 정책 설정 - logrotate

### logrotate란? - [https://linuxconfig.org/logrotate](https://linuxconfig.org/logrotate)

* 로그파일 교체 정도로 해석하면 될 듯
* log file들을 통제하는 리눅스 설정 및 명령어
* 명령어를 직접 사용하여 수동으로 실행할 수도 있음
* 설정파일을 생성하면, cron을 통해서 매일 지정한 설정대로 실행됩니다.

<br>

### logrotate 기능

1. 로그파일을 주기적으로 교체하는 기능 (매일, 매주, 매월, 매년)
2. 로그파일 압축저장 기능
3. 로그파일 메일전송 기능

크게 보면, 위의 3가지 기능 정도이고, 그 외 다양한 옵션들이 있습니다. - [https://linuxconfig.org/logrotate-8-manual-page](https://linuxconfig.org/logrotate-8-manual-page)

<br>

### logrotate 설정파일 작성

설정파일 경로: /etc/logrotate.d/
sudo vi [설정파일명]

```
/home1/irteam/logs/nginx/nginx-1.18.0/payco-shoppingnews-manager/*.log {
        daily
        rotate 30
        maxage 15
        missingok
        dateext
        sharedscripts
        create 640 irteam irteam
        postrotate
                if [ -f /home1/irteam/logs/nginx/nginx-1.18.0/nginx.pid ]; then
                        kill -USR1 `cat /home1/irteam/logs/nginx/nginx-1.18.0/nginx.pid`
                fi
        endscript
}
```

nginx 로그가 오랫동안 쌓여 서버의 저장소 용량을 많이 차지하기 않게함이 주 목적이기 때문에, 간단히 설정합니다.

* `/home1/irteam/logs/nginx/nginx-1.18.0/payco-shoppingnews-manager/*.log`: logrotate의 대상이 될 파일을 지정합니다.
* daily : 로그가 쌓이는 중인 파일의 이름을 바꾸어 저장합니다.
* create 640 irteam irteam: 새로 로그를 저장할 파일의 권한을 640으로 부여하고, 파일 소유자와 소유그룹을 irteam으로 지정합니다.
* rotate: 로그파일은 30개까지만 저장, 31개 이상부터는 오래된 로그순으로 제거됩니다.
* missingok: 위에서 지정한 logrotate의 대상 로그파일이 없어도 에러를 내지 않도록 합니다.
* dateext: 기존 로그파일의 이름을 바꿀 때, 파일명 뒤에 날짜 데이터를 붙입니다.
* postrotate \~ endscript: logrotate 작업 후, 실행할 shellscript를 지정합니다.
  * nginx는 USR1 을 받아야 파일을 새로 만듬
    [https://www.nginx.com/resources/wiki/start/topics/examples/logrotation/](https://www.nginx.com/resources/wiki/start/topics/examples/logrotation/)
* sharedscripts : postrotate에 지정한 shell script를 실행할 때, 위에서 지정한 logrotate의 대상이 될 파일들 모두에 적용하여 한번에 처리하도록 합니다. (access.log, error.log 모두 한번에 )