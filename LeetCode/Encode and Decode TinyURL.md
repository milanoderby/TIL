# Encode and Decode TinyURL

### 개념

TinyURL 시스템을 설계하는 문제입니다.

<br>

### 문제풀이

HashMap을 사용하여 문제를 해결합니다.

input 값으로 주어지는 URL 값의 해시코드를 생성하고, 이를 Map의 Key로 사용합니다.

해시코드를 사용하면, 바로 적절한 길이로 압축이 되기 때문에 이것이 바로 TinyURL이 됩니다.

<br>

나중에 TinyURL을 원래의 URL로 변환하는 값은 Map에서 Key를 이용하여 값을 가져오면 됩니다.

<br>

### 구현

```java
public class Codec {
    
    private static Map<String, String> tinyUrlMap = new HashMap<>();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        String shortUrlKey = longUrl.hashCode() + "";
        tinyUrlMap.putIfAbsent(shortUrlKey, longUrl);
        return shortUrlKey;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return tinyUrlMap.getOrDefault(shortUrl, "");
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

