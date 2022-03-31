# IntelliJ Code Style + Save Actions 적용

### Code Style 적용의 필요성

일관되지 않은 코드 스타일을 하나로 합쳐줍니다.

<br>

### Save Actions 적용의 이유

1. Code Style만 적용 후, 사용하려고 하면, 매번 Reformat 작업을 수동으로 해야하는 번거로움이 있음.

   Save Actions 적용 시, 코드를 저장할 때마다 Reformat 작업도 동시에 수행해줌.

2. Optimize Imports 기능 사용

   개발을 하다보면, 사용되지 않는 Import 문이 남아있게 되는데, 코드를 저장할 때마다 사용되지 않는 Import문을 제거하는 작업을 동시에 수행해줌.

<br>

### IntelliJ 에서 Kotlin Style guide 적용
1. 설정창을 띄움(Mac: `⌘` + `,` / Windows: `Ctrl` + `Alt` + `S`)

2. Editor > Code Style > Kotlin 으로 이동

3. `Set from...` 버튼 클릭

4. Kotlin style guide 선택

   ![image-20220330114422232](./images/code-style-1)

5. Scheme 을 Project 로 선택 후, Apply 버튼 클릭, 여기까지 하면 현재 프로젝트에 Code Style이 올바르게 적용된 것입니다.

  ![image-20220330114455569](./images/code-style-2)

6. Scheme 옆 톱니바퀴 클릭 > Copy To IDE 선택 (다른 Project 에서 적용하기 위해 설정을 IDE에 복사해둡니다.)

   ![image-20220330115228095](./images/code-style-3)

7. 새로운 Scheme 의 이름을 지정하고, Apply 버튼 클릭

   ![image-20220330115442948](./images/code-style-4)

8. 다른 Kotlin 프로젝트에서는 해당 Scheme을 선택하여 바로 Apply 하면, 적용됩니다.

참고자료: https://kotlinlang.org/docs/coding-conventions.html

### IntelliJ 에서 Java Style guide 적용

먼저, 첨부된 `intellij-java-google-style - modify indent.xml` 파일을 컴퓨터에 다운로드해주세요.

위 파일은 기존 google style guide 에서 tab과 space 키의 indent 값만 수정한 파일입니다.

1. 설정창을 띄움(Mac: `⌘` + `,` / Windows: `Ctrl` + `Alt` + `S`)

2. Editor > Code Style > Java 로 이동

3. Scheme 선택 창 옆 톱니바퀴 아이콘 클릭

4. Import Scheme 선택

5. IntelliJ IDEA code style XML 선택

   ![image-20220330114606723](./images/code-style-5)

6. 다운로드 받았던 `intellij-java-google-style - modify indent.xml` 파일을 선택

   ![image-20220330120000387](./images/code-style-6)

7. Scheme 을 Project 로 선택 후, Apply 버튼 클릭, 여기까지 하면 현재 프로젝트에 Code Style이 올바르게 적용된 것입니다.

  ![image-20220330120154279](./images/code-style-7)

8. Scheme 옆 톱니바퀴 클릭 > Copy To IDE 선택 (다른 Project 에서 적용하기 위해 설정을 IDE에 복사해둡니다.)

   ![image-20220330120425805](./images/code-style-8)

9. 새로운 Scheme 의 이름을 지정하고, Apply 버튼 클릭

   ![image-20220330120514669](./images/code-style-9)

10. 다른 Java 프로젝트에서는 해당 Scheme을 선택하여 바로 Apply 하면, 적용됩니다.

구글 스타일 가이드 repository: https://github.com/google/styleguide

### Save Actions 플러그인 적용

1. 설정창을 띄움(Mac: `⌘` + `,` / Windows: `Ctrl` + `Alt` + `S`)

2. Plugins > 상단 Marketplace 탭 > Save Actions 검색 및 설치

   ![image-20220330120726806](./images/code-style-10)

3. IntelliJ 재시작 후, 설정창을 띄움(Mac: `⌘` + `,` / Windows: `Ctrl` + `Alt` + `S`)

4. 메뉴 하단 Save Actions 클릭 및 사진처럼 체크

   ![image-20220330121430381](./images/code-style-11)

5. 최하단 적용 범위 선택

   Java 프로젝트면, java 파일에만 적용하므로 `.*\.java` 추가 

   Kotlin 프로젝트면, kt 파일에만 적용하므로 `.*\.kt` 추가

   ![image-20220330121624710](./images/code-style-12)