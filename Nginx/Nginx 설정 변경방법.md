# Nginx 설정 변경방법

1. nginx.conf 파일 변경

   `vi conf/nginx/nginx.conf`

2. 변경할 설정이 rate라고 가정 (rate=5000r/s로 변경)

   변경할 설정 검색 (vi 명령모드에서 `/rate` 로 변경할 설정 검색) 및 편집모드로  변경

3. rate 설정 재확인

   `cat /home/irteam/conf/nginx/nginx.conf | grep rate`

4. nginx 문법 확인

   `sudo /home/irteam/apps/nginx/sbin/nginx -t`

5. nginx 설정 적용

   `sudo /home/irteam/apps/nginx/sbin/nginx -s reload`
