# Maven pom.xml 의존성 추가가 안될 때

Maven 프로젝트에서 원격 Repository가 있고, 해당 repository 를 통해 특정 의존성을 가져올 때, 발생하는 문제입니다.

<br>

### 해결한 방법

Maven Repositories 설정에서 index되있는 원격 Maven Repository를 Update 해주었더니 의존성 위치가 식별되었습니다.

#### 우측 Maven 탭 > 도구 아이콘 > Maven Settings 클릭

![image-20211116145907234](./images/maven-dependency-not-added-1)

<br>

#### Build, Execution, Deployment > Build Tools > Maven > Repositories > 원격 Repository URL Update 버튼 클릭

![image-20211116144449781](./images/maven-dependency-not-added-2)

