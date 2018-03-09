# Configuration

## build.gralde

```groovy
buildscript {
    ext {
        springBootVersion = '1.5.6.RELEASE'
        thymeleafVersion = '3.0.1.RELEASE'
    }
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'

version = '0.0.1-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    compile("org.springframework.boot:spring-boot-starter-aop")
    compile("org.springframework.boot:spring-boot-starter-security")
    compile("org.springframework.boot:spring-boot-starter-data-jpa")
    compile("org.thymeleaf:thymeleaf:${thymeleafVersion}")
    compile("org.thymeleaf:thymeleaf-spring4:${thymeleafVersion}")
    compile("org.thymeleaf.extras:thymeleaf-extras-springsecurity4:${thymeleafVersion}")
    compile("net.sourceforge.nekohtml:nekohtml:1.9.22")
    runtime('com.h2database:h2')
//    runtime("mysql:mysql-connector-java")
    runtime("org.springframework.boot:spring-boot-devtools")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

## application.yml

```yml
#server:
#  port: 80

spring:
  profiles.active: logging-debug
  thymeleaf:
    mode: LEGACYHTML5
  h2:
    console:
      enabled: true
  datasource:
#    url: jdbc:mysql://192.168.1.35/idpravus?autoReconnect=true&useUnicode=true&characterEncoding=utf8
#    username: user
#    password: pwd
    url: jdbc:h2:mem:idpravus
    username: sa
    password:

  # JPA로 사용할 데이터베이스 명시
  jpa:
#    database: mysql
    database: h2
    show-sql: true
    hibernate:
      ddl-auto: update
#      ddl-auto: create-drop

---
spring.profiles: logging-debug
logging:
  file: logs/application.log
  level:
    org.thymeleaf: DEBUG
    org.springframework.web: DEBUG
    org.hibernate.SQL: DEBUG
    org.quartz.core: DEBUG

---
spring.profiles: logging-daily
logging:
  config: classpath:logback-spring.xml
```
