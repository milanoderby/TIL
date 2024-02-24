# Nginx ngx_http_geoip_module

ngx_http_geoip_module 모듈은 이미 컴파일된 MaxMind 데이터베이스를 사용하여 클라이언트 IP 주소에 따라 값을 갖는 변수를 생성합니다. 

- 이 모듈은 기본적으로 빌드되지 않으며 `--with-http_geoip_module` 구성 매개변수를 사용하여 활성화해야 합니다.

- 다만, `load_module 지시어`를 사용하여 아래와 같은 형태로 설정하면, 동적으로도 모듈을  연결하여 사용할 수 있습니다. 즉, 위와 같은 컴파일 방식을 사용하지 않아도 됩니다.

  ```nginx
  load_module "/etc/nginx/modules/ngx_http_geoip_module.so";
  ```

<br>

### 1. ngx_http_geoip_module 모듈 설정을 위해 필요한 패키지 설치

```nginx
# ngx_http_geoip_module.so 라는 동적 모듈을 설치
apt-get install nginx-module-geoip

# GeoLite2 무료 Geolocation DB를 다운로드하기 위해 설치
apt-get install wget

# GeoLite2 무료 Geolocation DB를 가공하는 명령어를 다운로드하기 위해 설치
apt-get install git

# GeoLite2 무료 Geolocation DB를 가공하는 명령어를 수행하기 위해 설치
apt-get install python3
```

<br>

### 2. 작업용 디렉토리 생성

앞으로의 모든 작업은 `/etc/nginx/geoip` 경로에서 이뤄질 것입니다.

디렉토리를 생성하고, 해당 경로로 이동합니다.

```shell
mkdir /etc/nginx/geoip && cd /etc/nginx/geoip
```

<br>

### 3. ngx_http_geoip_module 모듈 사용을 위해 GeoLite2 무료 Geolocation DB 다운로드

1. GeoLite2 회원가입

   - [회원가입](https://www.maxmind.com/en/geolite2/signup) 페이지에서 수행

   - 참고: https://dev.maxmind.com/geoip/geolite2-free-geolocation-data#accessing-geolite2-free-geolocation-data

2. `License Key` 생성

   - [내 계정 > 라이센스 키](https://www.maxmind.com/en/accounts/current/license-key) 페이지에서 수행

   - 참고: https://dev.maxmind.com/geoip/geolite2-free-geolocation-data#accessing-geolite2-free-geolocation-data

3. Geolocation DB 다운로드 메뉴 접근

   - [내 계정 > 파일 다운로드](https://www.maxmind.com/en/accounts/current/geoip/downloads) 메뉴 접근
   - 참고: https://dev.maxmind.com/geoip/geolite2-free-geolocation-data#accessing-geolite2-free-geolocation-data

4. Geolocation DB 다운로드를 위한 `Permalinks` 복사

   1. GeoLite2 Country: CSV Format > `Get Permalinks` > `# Database URL` 로 표시된 URL 복사

      ```
      https://download.maxmind.com/geoip/databases/GeoLite2-Country-CSV/download?suffix=zip
      ```

   2. GeoLite2 City: CSV Format > `Get Permalinks` > `# Database URL` 로 표시된 URL 복사

      ```
      https://download.maxmind.com/geoip/databases/GeoLite2-City-CSV/download?suffix=zip
      ```

5. `License Key`와 `Permalinks`를 이용한 GeoLite2 무료 Geolocation DB 다운로드

   - 아래는 `GeoLite2 무료 Geolocation DB 다운로드` 를 위한 명령어입니다.

     ```
     wget --content-disposition --user=YOUR_ACCOUNT_ID --password=YOUR_LICENSE_KEY 'PERMALINK'
     ```

   - `YOUR_ACCOUNT_ID` 값을 `account ID`로 치환하고,  `YOUR_LICENSE_KEY` 값을 `License key`로 치환하며, `PERMALINK` 값을 `Permalinks`의 값으로 치환하여 Geolocation DB 다운로드 명령어를 수행합니다.

   1. GeoLite2 Country: CSV Format 다운로드 명령어 형식

      ```shell
      wget --content-disposition --user=YOUR_ACCOUNT_ID --password=YOUR_LICENSE_KEY 'https://download.maxmind.com/geoip/databases/GeoLite2-Country-CSV/download?suffix=zip'
      ```

   2. GeoLite2 City: CSV Format 다운로드 명령어 형식

      ```shell
      wget --content-disposition --user=YOUR_ACCOUNT_ID --password=YOUR_LICENSE_KEY 'https://download.maxmind.com/geoip/databases/GeoLite2-City-CSV/download?suffix=zip'
      ```

<br>

### 4. GeoLite2 무료 Geolocation DB를 가공하는 명령어를 다운로드 및 이동

```shell
git clone https://github.com/sherpya/geolite2legacy.git
```

<br>

```shell
cp /etc/nginx/geoip/geolite2legacy/geolite2legacy.py /etc/nginx/geoip/ && cp /etc/nginx/geoip/geolite2legacy/geoname2fips.csv /etc/nginx/geoip/
```

<br>

### 5. GeoLite2 무료 Geolocation DB를 가공하는 명령어 수행

```shell
python3 geolite2legacy.py -i GeoLite2-Country-CSV_*.zip -f geoname2fips.csv -o GeoLiteCountry.dat && python3 geolite2legacy.py -i GeoLite2-City-*.zip -f geoname2fips.csv -o GeoLiteCity.dat
```

<br>

### 6. nginx conf 파일 수정

```nginx
vim /etc/nginx/nginx.conf 
```

<br>

#### 1. ngx_http_geoip_module 모듈 동적으로 연결

아래와 같이 최상위 컨텍스트에 load_module 지시어를 이용하여 ngx_http_geoip_module 모듈을 동적으로 연결합니다.

```
load_module "/etc/nginx/modules/ngx_http_geoip_module.so";

# ...

events {
	# ...
}

http {
	# ...
}
```

<br>

#### 2. 가공된 GeoLite2 무료 Geolocation DB 연동

`geoip_country 지시어` 와 `geoip_city 지시어`를 이용하여 아래와 위에서 가공한 GeoLite2 무료 Geolocation DB를 연동합니다.

```nginx
http {
	geoip_country /etc/nginx/geoip/GeoLiteCountry.dat;
    geoip_city /etc/nginx/geoip/GeoLiteCity.dat;
    
    # ...
}
```

<br>

### [ngx_http_geoip_module모듈에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_geoip_module.html)

#### geoip_country 관련

- `$geoip_country_code` : 2자리 국가 코드(예: "RU", "US")
- `$geoip_country_code3` : 3자리 국가 코드(예: "RUS", "USA")
- `$geoip_country_name` : 국가 이름(예: “Russian Federation”, “United States”)

<br>

#### geoip_city 관련

- `$geoip_city_continent_code` : 두 글자로 된 대륙 코드(예: "EU", "NA")
- `$geoip_city` : 도시 이름(예: "Moscow", "Washington")
- `$geoip_region_name` : 국가 지역 이름(지역, 영토, 주, 지방, 연방 토지 등) (예: “Moscow City”, “District of Columbia”.)
- `$geoip_postal_code` : 우편 번호
- `$geoip_latitude` : 위도
- `$geoip_longitude` : 경도