# Object Mapper Read Value 오류

```
Caused by: com.fasterxml.jackson.databind.JsonMappingException: No suitable constructor found for type [simple type, class com.nbp.nmp.spring.beans.factory.config.DatabaseInfo]: can not instantiate from JSON object (missing default constructor or creator, or perhaps need to add/enable type information?)
```

<br>

jackson ObjectMapper 를 이용하여 String을 Object 로 변환하려고할 때, 변환 대상이되는 Object의 **기본 생성자가 없으면** 오류가 발생합니다.

따라서, 다음과 같이 기본 생성자를 만들어주어야 합니다.

```java
public class DatabaseInfo {

    private String url;
    private String username;
    private String password;

	// 없으면 오류 발생!
    public DatabaseInfo() {}

    public DatabaseInfo(String url, String username, String password) {
        this.url = url;
        this.username = username;
        this.password = password;
    }

    public String getUrl() {
        return url;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }
}
```

<br>

ObjectMapper가 동작하는 원리에 대해서 공부해봅니다.

그렇다면, private 생성자로 할 때에도 문제가 발생하지 않을까요? 

네, 테스트해본 결과 그렇습니다. 이 결과로 보아 ObjectMapper의 원리는 Reflection과 관련이 있는 것 같습니다.

<br>

### 참고자료

