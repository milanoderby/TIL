# Java Map 인터페이스

Java Map 은 Key와 Value를 맵핑하는 자료구조입니다.

- Map은 중복된 Key를 가질 수 없습니다.
- 각각의 Key는 최대 1개의 Value를 가질 수 있습니다.
- Java의 Key나 Value는 불변 객체로 구성되어야합니다. (가변객체를 사용하여 추후 기존의 저장된 Key, Value와 새로 저장하려는 Key와 Value가 다를 경우, 원하던 값을 찾을 수 없으므로 오류가 발생할 수 있습니다.)
- Map 인터페이스의 몇몇 구현체들은 Key나 Value 값이 null로 인자로 들어오는 것을 금지합니다.
- Map 인터페이스의 몇몇 구현체들은 Key가 특정 타입인 것을 금지합니다.
- Map 인터페이스의 몇몇 구현체들은 insert 연산에서 금지하는 타입이나 값을 사용할 경우, unchecked Exception(NullPointerException 또는 ClassCastException)을 던집니다.
- Map 인터페이스의 몇몇 구현체들은 query 연산에서 금지하는 타입이나 값을 사용할 경우, Exception을 던지거나, false를 반환합니다.
- Collections Framework의 많은 인터페이스들이 `equals` 메소드에 기반하여 정의됩니다. 예를 들면, containsKey는 input key가 null 일 경우, Map 자료구조 내에 key가 null인 값을 찾으며, key가 null이 아닐 경우, key.equals() 연산을 했을 때의 동일한 Key가 존재하는지 여부를 반환합니다.

<br>

### Method - put 관련

| 수정자 및 반환타입 | 메소드명                                | 설명                                                         |
| ------------------ | --------------------------------------- | ------------------------------------------------------------ |
| V                  | put(K key, V value)                     | key를 value에 맵핑하며, 기존에 맵핑된 value를 반환합니다.<br />기존 key에 맵핑된 value가 없을 경우: 새로운 값으로 맵핑되고, 기존에 맵핑된 value가 없으므로 null을 반환합니다.<br />**기존 key에 맵핑된 value가 있을 경우: 새로운 값으로 맵핑되고, 기존에 맵핑된 value를 반환합니다.** |
| default V          | putIfAbsent(K key, V value)             | key를 value에 맵핑하며, 기존에 맵핑된 value를 반환합니다.<br />기존 key에 맵핑된 value가 없을 경우: 새로운 값으로 맵핑되고, 기존에 맵핑된 value가 없으므로 null을 반환합니다.<br />**기존 key에 맵핑된 value가 있을 경우: 기존 값으로 맵핑되고, 기존에 맵핑된 value를 반환합니다.** |
| void               | putAll(Map<? extends K, ? extends V> m) | 입력 값으로 받은 Map 내의 모든 연관관계를 모두 현재의 Map에 추가합니다. |

<br>

#### Map.put~ 메소드 예제

```java
public static void main(String[] args) throws IOException {
	Map<Integer, Integer> testMap = new HashMap<>();

    Integer oldValue = testMap.put(1, 2);
    Integer currentValue = testMap.get(1);
    System.out.println("oldValue: " + oldValue + ", currentValue :" + currentValue);

    oldValue = testMap.put(1, 3);
    currentValue = testMap.get(1);
    System.out.println("oldValue: " + oldValue + ", currentValue :" + currentValue);

    oldValue = testMap.putIfAbsent(1, 4);
    currentValue = testMap.get(1);
    System.out.println("oldValue: " + oldValue + ", currentValue :" + currentValue);

    oldValue = testMap.putIfAbsent(2, 4);
    currentValue = testMap.get(2);
    System.out.println("oldValue: " + oldValue + ", currentValue :" + currentValue);

    Map<Integer, Integer> copiedMap = new HashMap<>();
    copiedMap.putAll(testMap);
    currentValue = testMap.get(1);
    System.out.println("currentValue of copiedMap when key is 1:" + currentValue);
}

// 출력결과
oldValue: null, currentValue :2
oldValue: 2, currentValue :3
oldValue: 3, currentValue :3
oldValue: null, currentValue :4
currentValue of copiedMap when key is 1:3
```

<br>

### Method - get 관련

| 수정자 및 반환타입 | 메소드명                                 | 설명                                                         |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| V                  | get(Object key)                          | key에 맵핑된 value를 반환합니다.<br />**key에 맵핑된 value가 없을 경우: 기존에 맵핑된 value가 없으므로 null을 반환합니다.**<br />key에 맵핑된 value가 있을 경우: 기존에 맵핑된 value를 반환합니다.<br />value로 null 값이 허용될 경우, get으로 null을 반환받았다해도 Map 안의 key 자체가 없는 것을 의미하는 것은 아닙니다. key는 있으나, key에 저장된 값이 null일 경우가 생기는 것이니까요. 이 때는, key 자체는 존재하는지 구분하기 위해 containsKey 메소드를 사용해야합니다. |
| default V          | getOfDefault(Object key, V defaultValue) | key에 맵핑된 value를 반환합니다.<br />**key에 맵핑된 value가 없을 경우: 기존에 맵핑된 value가 없으면, defaultValue를 반환합니다.**<br />key에 맵핑된 value가 있을 경우: 기존에 맵핑된 value를 반환합니다.<br />기본 구현체는 동기화나 원자성을 보장하지 않습니다. 동기화가 필요할 경우, 인터페이스를 직접 구현하여야 합니다. |

<br>

#### Map.get~ 메소드 예제

```java
public static void main(String[] args) throws IOException {
	Map<Integer, Integer> testMap = new HashMap<>();

    Integer currentValue = testMap.get(1);
    System.out.println("currentValue :" + currentValue);

    currentValue = testMap.getOrDefault(1, 2);
    System.out.println("currentValue :" + currentValue);

    testMap.put(1, 3);
    currentValue = testMap.get(1);
    System.out.println("currentValue :" + currentValue);

    currentValue = testMap.getOrDefault(1, 2);
    System.out.println("currentValue :" + currentValue);
}

// 출력결과
currentValue :null
currentValue :2
currentValue :3
currentValue :3
```

<br>

### Method - 반복 관련

| 수정자 및 반환타입   | 메소드명                                        | 설명                                                         |
| -------------------- | ----------------------------------------------- | ------------------------------------------------------------ |
| default void         | forEach(BiConsumer<? super K,? super V> action) | 기본 구현은 다음과 같습니다.<br />`for (Map.Entry<K, V> entry : map.entrySet())      action.accept(entry.getKey(), entry.getValue());`<br />즉, 반복적으로 인자로 받은 lambda function을 수행합니다. |
| Set<Map.Entry<K, V>> | entrySet()                                      | Map에 저장되어있는 모든 연관관계를 Set View로 반환합니다. Map의 Key, Value가 변경될 경우, Set 내부의 값도 변경됩니다. 역으로, Set 내부의 값이 변경될 경우, Map의 Key, Value도 변경됩니다. (단, Iterator를 이용한 반복문 내에서 remove 연산, setValue 연산으로 수정한 경우는 제외입니다.)<br />Set 에서 일반적인 제거 연산도 가능합니다. (Iterator.remove, Set.remove, removeAll, retainAll)<br />반면, add나 addAll 같은 추가 연산은 지원하지 않습니다. |
| Set<K>               | keySet()                                        | Map에 저장되어있는 모든 Key를 Set View로 반환합니다. Map의 Key가 변경될 경우, Set 내부의 Key 값도 변경됩니다. 역으로, Set 내부의 Key 값이 변경될 경우, Map의 Key 값도 변경됩니다. (단, Iterator를 이용한 반복문 내에서 remove 연산으로 수정한 경우는 제외입니다.)<br />Set 에서 일반적인 제거 연산도 가능합니다. (Iterator.remove, Set.remove, removeAll, retainAll)<br />반면, add나 addAll 같은 추가 연산은 지원하지 않습니다. |

<br>

#### Map 반복문 관련 예제

```java
public static void main(String[] args) throws IOException {
	Map<Integer, Integer> testMap = new HashMap<>();

    testMap.put(1, 11);
    testMap.put(2, 12);
    testMap.put(3, 13);

    final List<Integer> anyValueList = new ArrayList<>();
    testMap.forEach((key, value) -> {
        System.out.println("key: " + key + ", value: " + value);

        // 아래는 실제 value가 변하지 않습니다.
        value += 100;

        // 아래는 컴파일 불가능한 연산, Variable used in lambda expression should be final or effectively final
        // anyValue = value;

        // 아래는 컴파일 가능한 연산, List가 final로 선언되어있어도 값은 저장할 수 있기 때문입니다.
        anyValueList.add(testMap.get(key));
    });

    for (Entry<Integer, Integer> entry : testMap.entrySet()) {
        System.out.println("key: " + entry.getKey() + ", value: " + entry.getValue());

        // 아래는 실제 value가 변합니다.
        entry.setValue(entry.getValue() + 100);

        // 아래는 컴파일 불가능한 연산, Variable used in lambda expression should be final or effectively final
        // anyValue = entry.getValue();

        // 아래는 컴파일 가능한 연산, List가 final로 선언되어있어도 값은 저장할 수 있기 때문입니다.
        anyValueList.add(entry.getValue());
    }

    for (Integer key : testMap.keySet()) {
        System.out.println("key: " + key + ", value: " + testMap.get(key));

        // 아래는 실제 value가 변합니다.
        testMap.put(key, testMap.get(key) + 100);

        // 아래는 컴파일 불가능한 연산, Variable used in lambda expression should be final or effectively final
        // anyValue = testMap.get(key);

        // 아래는 컴파일 가능한 연산, List가 final로 선언되어있어도 값은 저장할 수 있기 때문입니다.
        anyValueList.add(testMap.get(key));
    }

    testMap.forEach((key, value) -> {
        // 위에서 변경한 값들 출력
        System.out.println("key: " + key + ", value: " + value);
    });

    for (int index = 0; index < anyValueList.size(); index++) {
        System.out.println("index(" + index + "): " + anyValueList.get(index));
    }
}

// 출력결과
key: 1, value: 11
key: 2, value: 12
key: 3, value: 13
key: 1, value: 11
key: 2, value: 12
key: 3, value: 13
key: 1, value: 111
key: 2, value: 112
key: 3, value: 113
key: 1, value: 211
key: 2, value: 212
key: 3, value: 213
index(0): 11
index(1): 12
index(2): 13
index(3): 111
index(4): 112
index(5): 113
index(6): 211
index(7): 212
index(8): 213
```

<br>

### Method - compute 관련

| 수정자 및 반환타입 | 메소드명                                                     | 설명                                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| default V          | compute(K key,                   BiFunction<? super K,? super V,? extends V> remappingFunction) | **입력 값으로 주어진 Key와 해당 Key에 연관되어있는 Value 그리고, remappingFunction을 이용하여 새로 계산된 값을 Key에 맵핑합니다. 새로 맵핑된 Value를 반환합니다.<br />**key에 맵핑된 기존 value가 없을 경우: remappingFunction 에서 인자로 받는 value는 null로 치환됩니다. remappingFunction으로 계산한 값이 새로운 value가 됩니다.<br />key에 맵핑된 기존 value가 있을 경우: **remappingFunction 에서 인자로 받는 value는 기존 value로 치환됩니다. remappingFunction으로 계산한 값이 새로운 value가 됩니다.**<br />**remappingFunction에 의해 새로 계산된 newValue가 null일 경우, 기존 맵핑은 제거됩니다.**<br />만약, remappingFunction이 Exception을 던진다면, Map에는 어떤 값도 return이 되지 않으므로 value 값에도 변화가 발생하지 않습니다.<br />만약, 기존에 값이 없을 때에는 defaultValue를 맵핑하고 싶고, 기존에 값이 있을 때에는 해당 값에 연산을 한 값으로 맵핑하고 싶다면, 아래와 같이 수행하면 됩니다.<br />`map.compute(key, (k, v) -> (v == null) ? msg : v.concat(msg))` |
| default V          | computeIfAbsent(K key,                           Function<? super K,? extends V> mappingFunction) | **입력 값으로 주어진 Key에 연관되는 Value가 없을 때에만 remappingFunction을 이용하여 새로 계산된 값을 Key에 맵핑합니다. 현재 Key에 맵핑되어있는 기존 Value 또는 방금 계산된 Value를 반환합니다.**<br />key에 맵핑된 기존 value가 없을 경우: remappingFunction 에서 인자로 받는 value는 null로 치환됩니다. remappingFunction으로 계산한 값이 새로운 value가 됩니다.<br />**key에 맵핑된 기존 value가 있을 경우: 기존 value만 반환하고, remapping은 발생하지 않습니다.**<br />**remappingFunction에 의해 새로 계산된 newValue가 null일 경우, 기존 맵핑은 유지됩니다.**<br />만약, remappingFunction이 Exception을 던진다면, Map에는 어떤 값도 return이 되지 않으므로 value 값에도 변화가 발생하지 않습니다.<br />만약, 기존에 값이 없을 때에만 defaultValue를 맵핑하고 싶다면, 아래와 같이 수행하면 됩니다.<br />`map.compute(key, (k, v) -> msg)` |
| default V          | computeIfPresent(K key,                            BiFunction<? super K,? super V,? extends V> remappingFunction) | **입력 값으로 주어진 Key에 연관되는 Value가 있고, null이 아닐 때에만 remappingFunction을 이용하여 새로 계산된 값을 Key에 맵핑합니다. 방금 계산된 Value를 반환하고, 기존 Key에 연관된 값이 없었으면, null을 반환합니다.**<br />**key에 맵핑된 기존 value가 없을 경우: null 만 반환하고, remapping은 발생하지 않습니다. **<br />**key에 맵핑된 기존 value가 있을 경우: remappingFunction 에서 인자로 받는 value는 기존 key에 맵핑된 value로 치환됩니다. remappingFunction으로 계산한 값이 새로운 value가 됩니다.**<br />**remappingFunction에 의해 새로 계산된 newValue가 null일 경우, 기존 맵핑은 유지됩니다.**<br />만약, remappingFunction이 Exception을 던진다면, Map에는 어떤 값도 return이 되지 않으므로 value 값에도 변화가 발생하지 않습니다.<br />만약, 기존에 값이 있을 때에만 defaultValue를 맵핑하고 싶다면, 아래와 같이 수행하면 됩니다.<br />`map.compute(key, (k, v) -> msg)` |

<br>

#### Map.compute~ 관련 예제

```java
public static void main(String[] args) throws IOException {
	Map<Integer, Integer> testMap = new HashMap<>();

    testMap.put(1, 11);
    testMap.put(2, 12);
    testMap.put(3, 13);
    
    
}

// 출력결과
```

<br>



## 참고자료

https://docs.oracle.com/javase/8/docs/api/java/util/Map.html