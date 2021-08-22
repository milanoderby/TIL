# gitignore.io 사용법

`.gitignore` 파일은 git에 push 할 때, 변경을 감지하지 않을 파일들을 명시하는 파일입니다.

<br>

### gitignore.io

gitignore.io 사이트는 .gitignore 파일을 생성해주는 사이트입니다.

https://www.toptal.com/developers/gitignore 사이트를 들어가서 주요 개발환경을 입력하면, 그에 맞는 .gitignore 파일을 생성합니다.

저는 IntelliJ, Java, Gradle 을 사용하여 개발을 하기 때문에 아래와 같이 입력하였습니다.

![image-20210822113425369](./images/gitignore-1)

<br>

### 생성된 .gitignore 파일

```

# Created by https://www.toptal.com/developers/gitignore/api/intellij+all,java,gradle
# Edit at https://www.toptal.com/developers/gitignore?templates=intellij+all,java,gradle

### Intellij+all ###
# Covers JetBrains IDEs: IntelliJ, RubyMine, PhpStorm, AppCode, PyCharm, CLion, Android Studio, WebStorm and Rider
# Reference: https://intellij-support.jetbrains.com/hc/en-us/articles/206544839

# User-specific stuff
.idea/**/workspace.xml
.idea/**/tasks.xml
.idea/**/usage.statistics.xml
.idea/**/dictionaries
.idea/**/shelf

# AWS User-specific
.idea/**/aws.xml

# Generated files
.idea/**/contentModel.xml

# Sensitive or high-churn files
.idea/**/dataSources/
.idea/**/dataSources.ids
.idea/**/dataSources.local.xml
.idea/**/sqlDataSources.xml
.idea/**/dynamic.xml
.idea/**/uiDesigner.xml
.idea/**/dbnavigator.xml

# Gradle
.idea/**/gradle.xml
.idea/**/libraries

# Gradle and Maven with auto-import
# When using Gradle or Maven with auto-import, you should exclude module files,
# since they will be recreated, and may cause churn.  Uncomment if using
# auto-import.
# .idea/artifacts
# .idea/compiler.xml
# .idea/jarRepositories.xml
# .idea/modules.xml
# .idea/*.iml
# .idea/modules
# *.iml
# *.ipr

# CMake
cmake-build-*/

# Mongo Explorer plugin
.idea/**/mongoSettings.xml

# File-based project format
*.iws

# IntelliJ
out/

# mpeltonen/sbt-idea plugin
.idea_modules/

# JIRA plugin
atlassian-ide-plugin.xml

# Cursive Clojure plugin
.idea/replstate.xml

# Crashlytics plugin (for Android Studio and IntelliJ)
com_crashlytics_export_strings.xml
crashlytics.properties
crashlytics-build.properties
fabric.properties

# Editor-based Rest Client
.idea/httpRequests

# Android studio 3.1+ serialized cache file
.idea/caches/build_file_checksums.ser

### Intellij+all Patch ###
# Ignores the whole .idea folder and all .iml files
# See https://github.com/joeblau/gitignore.io/issues/186 and https://github.com/joeblau/gitignore.io/issues/360

.idea/

# Reason: https://github.com/joeblau/gitignore.io/issues/186#issuecomment-249601023

*.iml
modules.xml
.idea/misc.xml
*.ipr

# Sonarlint plugin
.idea/sonarlint

### Java ###
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

### Gradle ###
.gradle
build/

# Ignore Gradle GUI config
gradle-app.setting

# Avoid ignoring Gradle wrapper jar file (.jar files are usually ignored)
!gradle-wrapper.jar

# Cache of project
.gradletasknamecache

# # Work around https://youtrack.jetbrains.com/issue/IDEA-116898
# gradle/wrapper/gradle-wrapper.properties

### Gradle Patch ###
**/build/

# Eclipse Gradle plugin generated files
# Eclipse Core
.project
# JDT-specific (Eclipse Java Development Tools)
.classpath

# End of https://www.toptal.com/developers/gitignore/api/intellij+all,java,gradle
```

