## Java

### 1. 변수형에서 다른 언어와 다른 점

- 자바 변수 별 기본 값 대부분은 c와 같으나,
  `char` 형의 기본 값은 스페이스 문자를 의미하는 `\u0000` 이다.
  `char ch = ' ';`로 초기화 되어있음

- 참조 자료형은 null로 초기화 되어있음

  `String ref = null;`



### 2. 파일 입출력

##### InputStream의 종류 : ByteStream(1바이트씩 읽어옴) / CharStream(2바이트씩 읽어옴)

- `FileInputStream` : `ByteStream 계열`

  ```java
  FileInputStream in = new FileInputStream("C:/dev/test.txt");
  in.read(); //1바이트씩 읽어오는 방식
  
  char bytes[1024];
  in.read(bytes); //1024바이트씩 읽어오는 방식
  ```

- 위와 같은 ByteStream으로 문자를 읽어오면, 한글과 같이 2바이트로 인코딩 되어 있는 문자들은 깨져서 데이터를 읽어오게 된다. 이를 해결하기 위해, 한줄씩 받아와야 된다. 그런데, 한줄씩 읽어올 때 사용하는 `inputstream`인 `BufferedReader`는 `CharStream 계열` 이므로 `FileInputStream`과 직접 호환되지 않으므로, 중간 보조 스트림인 `InputStreamReader`를 사용해서, `BufferedReader`와 연결한다.

- `InputStreamReader` : 한줄씩 읽어올 때, 사용하는 보조스트림

  ```java
  InputStreamReader isr = new InputStreamReader(in, "utf-8");
  // FileInputStream 변수인 in에서 "UTF-8"로 읽어온다.
  ```

- `BufferedReader` : 한줄씩 문자열을 읽어올 때, 사용하는 `CharStream`

  ```java
  BufferedReader reader = new BufferedReader(isr);
  ```

- `File` 대신에 `웹사이트의 소스코드`를 가져오고 싶다면,
  `FileInputStream` 대신 다음과 같이 `InputStream`으로 가져와서 `in`이라는 변수에 저장하고,  `InputStreamReader`에 `in`을 넘겨주면 된다.

  

### 3. Java 웹 크롤링

- 위의 inputStream을 이용하여 web page의 소스코드를 가져온다.

  ```java
  // 서버URL 접속 라이브러리 사용
  URL url = new URL("http://ggoreb.com/http/html2.jsp");
  URLConnection con = url.openConnection();
  
  // 인터넷 웹사이트 내용을 가져오는 스트림
  InputStream in = con.getInputStream();
  
  // InPutStreamReader라는 보조스트림을 이용하여 InputStream과 BufferedReader를 연결
  InputStreamReader isr = new InputStreamReader(in, "utf-8");
  
  // 한줄씩 읽어오기 위해 BufferedReader를 생성
  BufferedReader reader = new BufferedReader(isr);
  
  // response를 읽어오기 위해 StringBuffer 객체를 생성
  StringBuffer response = new StringBuffer();
  
  
  ```

- `JSON` : 서버에서 클라이언트로 data를 넘겨줄 때, 사용하는 형식 중 하나

  지금은 웹에서 가져오는 data는 JSON형식 이므로 `JSON library`를 다운로드 받는다.

  `http://mavenrepository.com`에서 JSON 검색 후, 최신 버전 들어간 다음, first bundle을 누르면, JSON jar파일 다운로드 된다. Eclipse에서 add jar를 이용하여 `JSON library`를 이용할 수 있도록 설정한다.

- 



## 4. 예외처리

- ### 예외처리 방법

  `throws exception`은 문제가 발생한 `method`를 호출한 `method`에게 책임을 전가하는 의미를 가진다. 계속해서 호출한 `method`에 `throws exception` 하다가 `try`~`catch` 문이 있는 `method`에서는 예외를 처리해준다.

- ### `Console` 창에서 발생한 `Exception Code`를 보고, `Exception`이 발생한 부분을 추적하는 방법

  예외가 발생하면, 예외가 발생한 메소드부터 그 메소드를 호출한 메소드들이 순차적으로 StackTrace에 출력이 된다. 이 경로를 차례대로 추적하다가 내가 작업한 메소드를 발견하고, 예외를 해결하면 된다.

- ### 예외처리 2 종류 (언제 어떤 방식을 사용하는가)
  1. `throws exception` : 다른 사람이 사용할 라이브러리를 만드는 사람이면 항상 예외를 던져야 한다. 만약, 라이브러리의 메소드를 사용하는 사람 입장에서 내부 `method`가 `try` ~ `catch`로 처리되어 있어서 이미 작업이 마무리 되고, 결과가 변하지 않는다면, 라이브러리를 가져다 쓰는 사람들은 왜 안되는지 이유를 알 수 없기 때문이다. 
  2. `try` ~ `catch` : 위의 상황과 반대로 내가 라이브러리를 이용하여 코드를 짜는 가장 최종적인 개발자의 입장이라면 내가 모든 상황을 이해하고 있기 때문에 try ~ catch문을 사용한다.




## 5. `Generic`

- ### `Generic class`

  자료구조를 Java에서 구현할 때, 다양한 자료형을 담을 수 있게 설계한 class

  다양한 자료형에 대해서 같은 operation을 사용할 수 있다.

  

- ### `Collection` 자료구조

  Collection 자료구조를 사용할 때, Java는 기존의 자료형을 사용할 수 없고, `Wrapper Class`를 사용할 수 있다.

  `Wrapper Class` 종류 : `Boolean`, `Character`, `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`,

  ```java
  ArrayList<Integer> myArr = new ArrayList<>();	//ArrayList Collection 사용예제
  HashSet<String> mySet = new HashSet<>();		//HashSet Collection 사용예제
  HaspMap<String, Object> myMap = new HashMap<>();//HashMap Collection 사용예제
  ```

  

## 6. `String` 관련 `method`

- ### 검색

  `indexOf` :

- ### 바꾸기

  `replace` : 

- ### 잘라내기

  `substring` : 

