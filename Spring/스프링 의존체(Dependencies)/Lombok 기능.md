# Lombok 기능

자주 쓰이고, 유용한 것에 대해서만 설명합니다.



`@Getter/@Setter`: 자바 객체의 접근자, 설정자 메소드를 생성해줍니다. `@Setter` 는 사용을 자제합니다. (불변객체가 아니기 때문에)

`@With`: `@Setter` 대신, 사용을 권장하는 애노테이션입니다. `@With` 를 설정한 필드의 값은 `with~()` 메소드로 값을 변경할 수 있으며, 불변객체를 반환합니다.

`@Data`: 클래스 내 모든 필드에 `@ToString`, `EqualsAndHashCode`, `Getter`를 설정하고, final이 아닌 필드에 `@Setter`를 설정하며, `RequiredArgsConstructor`를 생성해줍니다. - 왠만하면 사용을 자제합니다. (불변객체가 아니기 때문에)

`@Builder`: `AllArgsConstructor`와 각각의 변수 값 설정자메소드를 가지는 `내부 static 클래스`를 가지고 있습니다. 

- `builder` : Builder용 객체를 반환
- 각 변수이름으로 하는 메소드: Setter 메소드 (Reflection을 이용하여야 만들 수 있는 메소드)
- `build` : Builder 용 객체 변수 값들을 `AllArgsConstructor` 의 인자로 전달하여 호출합니다.

```java
import java.util.Set;

public class BuilderExample {
  private long created;
  private String name;
  private int age;
  private Set<String> occupations;
  
  BuilderExample(String name, int age, Set<String> occupations) {
    this.name = name;
    this.age = age;
    this.occupations = occupations;
  }
  
  private static long $default$created() {
    return System.currentTimeMillis();
  }
  
  public static BuilderExampleBuilder builder() {
    return new BuilderExampleBuilder();
  }
  
  public static class BuilderExampleBuilder {
    private long created;
    private boolean created$set;
    private String name;
    private int age;
    private java.util.ArrayList<String> occupations;
    
    BuilderExampleBuilder() {
    }
    
    public BuilderExampleBuilder created(long created) {
      this.created = created;
      this.created$set = true;
      return this;
    }
    
    public BuilderExampleBuilder name(String name) {
      this.name = name;
      return this;
    }
    
    public BuilderExampleBuilder age(int age) {
      this.age = age;
      return this;
    }
    
    public BuilderExampleBuilder occupation(String occupation) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }
      
      this.occupations.add(occupation);
      return this;
    }
    
    public BuilderExampleBuilder occupations(Collection<? extends String> occupations) {
      if (this.occupations == null) {
        this.occupations = new java.util.ArrayList<String>();
      }

      this.occupations.addAll(occupations);
      return this;
    }
    
    public BuilderExampleBuilder clearOccupations() {
      if (this.occupations != null) {
        this.occupations.clear();
      }
      
      return this;
    }

    public BuilderExample build() {
      // complicated switch statement to produce a compact properly sized immutable set omitted.
      Set<String> occupations = ...;
      return new BuilderExample(created$set ? created : BuilderExample.$default$created(), name, age, occupations);
    }
    
    @java.lang.Override
    public String toString() {
      return "BuilderExample.BuilderExampleBuilder(created = " + this.created + ", name = " + this.name + ", age = " + this.age + ", occupations = " + this.occupations + ")";
    }
  }
}
```



`@ToString`: 자바 객체의 각 필드와 필드 값을 반환하는 toString 메소드를 생성해줍니다. 디버깅할 때, 유용합니다.

`@EqualsAndHashCode`: 자바 객체의 `hashCode` 값을 생성해주며, 이 값을 이용하여 동등 비교연산을 하는 `equals` 메소드를 생성해줍니다.

`@NonNull`: 필드 또는 메소드의 인자에 붙이는 애노테이션으로, Null Check를 진행합니다.

`@NoArgsConstructor`: 인자가 없는 생성자를 생성해줍니다.

`@RequiredArgsConstructor`: 초기화되지 않은 필드 중 `final` 필드나`@NonNull` 애노테이션이 붙은 필드의 값을 생성자의 인자로 대입하는 생성자를 생성해줍니다. 즉, required란, 생성자로 반드시 값을 초기화해줘야하는 필드를 의미합니다.

`@AllArgsConstructor`: 각 필드의 값을 생성자의 인자로 대입하는 생성자를 생성해줍니다.



## 참고자료

- https://projectlombok.org/features/all

