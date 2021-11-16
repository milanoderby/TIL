# Maven pom.xml 의존성 추가가 안될 때

`pom.xml` 파일에서 의존성 추가를 했음에도 반영이 되지 않는 경우가 있습니다.

보통 해당 dependency를 찾지 못했을 때 발생하는 문제입니다.

<br>

### 해결한 방법

이것 때문인지는 알 수 없지만, Maven Repositories 설정에서 index되있는 원격 Maven Repository를 Update 해주었더니 의존성 위치가 식별되었습니다.

#### 우측 Maven 탭 > 도구 아이콘 > Maven Settings 클릭

![image-20211116145907234](./images/maven-dependency-1)

<br>

#### Build, Execution, Deployment > Build Tools > Maven > Repositories > 원격 Repository URL Update 버튼 클릭

![image-20211116144449781](./images/maven-dependency-2)

