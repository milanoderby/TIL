# AWS EC2 인스턴스

1. 프리티어 가입 - 보고 따라하면 됩니다. - 마지막에 무료 사용
2. 우측 상단에 지역을 `Seoul/Asia Pacific` 으로 변경
3. 좌측 메뉴 Instances 클릭 - `t2.micro` (1GB 메모리) 생성
   - 마지막에 security key pair 새로 생성
4. putty로 aws instance 접속
   - https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html
   - 위에서 다운받은 `.pem` 파일을 putty용 `.ppk` 파일로 변환 - puttygen 프로그램 이용
   - https://docs.toast.com/ko/Compute/Instance/ko/overview/#_5
   - 위의 `.ppk` 파일을 putty Auth 설정 개인키 파일에 추가
   - 접속하면, 기본 username은 `centos`로 설정되어있음

