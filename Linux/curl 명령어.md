# curl 명령어

### 사용법

curl [option] URL

<br>

### 자주 사용하는 option

| option                         | 설명                                                         | 사용법 예시                                          |
| ------------------------------ | ------------------------------------------------------------ | ---------------------------------------------------- |
| -X [HTTP Method명]             | 사용할 HTTP Method를 지정합니다.                             | curl -X GET www.naver.com                            |
| -H [헤더명: 값]                | Request의 추가 Header를 지정합니다                           | curl -H Content-Type: application/json www.naver.com |
| -s<br />--silent               | curl 명령어의 출력을 standard output 으로 내보내지 않습니다.<br />대신, 다른 스트림으로 출력하는 명령어는 유효합니다. | curl -s www.naver.com                                |
| -S<br />--show-error           | 에러 발생 시, 에러를 출력하는 옵션입니다.                    |                                                      |
| -I                             | Response의 Header 값만 가져옵니다.                           | curl -I www.naver.com                                |
| -k<br />--insecure             | 안전하지 않은 (인증되지 않은)서버로의 연결을 수행합니다.     | curl -k http://www.naver.com                         |
| -o [file]<br />--output [file] | 실행결과를 file에 출력합니다.                                | curl -o result.txt www.naver.com                     |

<br>

### 활용 - 응답코드를 파일에 저장

`curl -I https://naver.com | grep HTTP > test.txt`

<br>

### bash 에서 curl 명령어를 이용한 GET 호출이 잘 되지 않을 때
이럴 경우, 문제점은 bash에서 `&` 나 `=` 과 같은 문자가 제대로 인식되지 않아서 발생하는 문제일 확률이 높습니다.

아래와 같은 방법을 이용하여 문제를 해결합니다.

`curl -X GET -G http://example.com -d var1=$1 -d var2=$2`

### 참고자료

https://curl.se/docs/manpage.html

http://www.mit.edu/afs.new/sipb/user/ssen/src/curl-7.11.1/docs/curl.html
