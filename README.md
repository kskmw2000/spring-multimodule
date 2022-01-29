# Spring Multi Module 생성하기

---

### Spring initialize 로 Spring Boot Project를 생성합니다.
- Gradle Porject 로 생성합니다.
  - Intellij의 Ultimate Version을 통해서 생성
  - start.spring.io 을 통해서 프로젝트를 생성

### 프로젝트의 오른쪽 마우스를 클릭하여 module을 추가합니다.
- gradle을 선택하시고, 모듈명을 입력한 후 finish 버튼을 클릭하여 모듈을 생성합니다.
- module project가 하나 생깁니다.

### root의 gradle 파일 성명
```root의 gradle파일
buildscript {
    ext {
        springBootVersion = '2.6.3'
    }
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
        classpath "io.spring.gradle:dependency-management-plugin:1.0.11.RELEASE"
    }
}

allprojects {
    group 'me.angeloper'
    version '1.0-SNAPSHOT'
}

subprojects {
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
    apply plugin: 'io.spring.dependency-management'

    sourceCompatibility = 11

    repositories {
        mavenCentral()
    }

    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
        implementation 'org.springframework.boot:spring-boot-starter-web'
        compileOnly 'org.projectlombok:lombok'
        developmentOnly 'org.springframework.boot:spring-boot-devtools'
        runtimeOnly 'com.h2database:h2'
        annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
        annotationProcessor 'org.projectlombok:lombok'
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }
}

project(':module-master-api') {
    dependencies {
        implementation project(':module-common')
    }
}
```

### 공통 모듈의 gralde 파일
- 주요 사항 : 공통 모듈의 경우에는 실행으로 되어야 할 `bootJar`는 활성화가 되지 말아야 하며, `jar`의 경우에는 다른 모듈에서 import 해야 하므로 활성화 시켜야 합니다.
```gradle
bootJar {
    enabled = false
}

jar {
    enabled = true
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}
```

### 다른 모듈의 gradle 파일
```gradle
plugins {
    id 'java'
}

group 'me.angeloper'
version '0.0.1-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

test {
    useJUnitPlatform()
}
```

---

### 궁금점
- `module-common`에 존재하는 entity는 jpa에서 자동으로 만들어 지는 부분이 없는가에 궁금증이 있는데 찾을수가 없다.