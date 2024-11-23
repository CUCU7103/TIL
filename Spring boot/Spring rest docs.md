# Spring Rest Docs

- **Spring Rest Docs는 Spring MVC 기반** 애플리케이션을 위한 API 문서화를 자동화하는 라이브러리입니다

- **라이브러리는 테스트 케이스와 연동**되어 API의 입력 및 출력을 문서화하므로, 항상 최신 상태의 문서를 유지할 수 있습니다.

  - 문서를 작성하기 위해서는 반드시 테스트 코드를 작성해야 합니다.

    

**주요 특징:**

- **테스트 코드 기반 문서화**
- Asciidoctor 형식 사용
- 스니펫(snippets) 생성을 통한 모듈화된 문서 작성



gradle 설정하기

```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.11-SNAPSHOT'
    id 'io.spring.dependency-management' version '1.1.6'
    id "org.asciidoctor.jvm.convert" version "3.3.2" // asciidoctor 지정
    id 'jacoco'
}

group = 'spring'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

configurations {
    asciidoctorExtensions
 
}



dependencies {
  
    // build/generated-snippets 에 생긴 .adoc 조각들을 프로젝트 내의 .adoc 파일에서 읽어들일 수 있도록 연동해줍니다.
    // 이 덕분에 .adoc 파일에서 operation 같은 매크로를 사용하여 스니펫 조각들을 연동할 수 있는 것입니다.
    // 그리고 최종적으로 .adoc 파일을 HTML로 만들어 export 해줍니다.
    asciidoctorExtensions 'org.springframework.restdocs:spring-restdocs-asciidoctor'

    // restdocs-mockmvc의 testCompile 구성 -> mockMvc를 사용해서 snippets 조각들을 뽑아낼 수 있게 된다.
    // MockMvc 대신 WebTestClient을 사용하려면 spring-restdocs-webtestclient 추가
    // MockMvc 대신 REST Assured를 사용하려면 spring-restdocs-restassured 를 추가
    testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'


}

ext {
    // snippetsDir 변수를 build/generated-snippets 디렉토리로 설정합니다.
    // 이 디렉토리는 테스트 중에 생성된 문서 스니펫 파일들이 저장되는 위치
    snippetsDir = file('build/generated-snippets')
}

test {
    // 테스트 실행 시 생성된 스니펫 파일들을 snippetsDir에 출력합니다.
    outputs.dir snippetsDir
    finalizedBy 'jacocoTestReport'
    useJUnitPlatform()
}

asciidoctor {
    // asciidoctor 작업은 test 작업이 완료된 후 실행됩니다. 이는 스니펫 파일들이 먼저 생성되어야 하기 때문입니다.
    dependsOn test
    // Asciidoctor 확장 기능을 위한 의존성을 설정합니다.
    configurations 'asciidoctorExtensions'
    // 문서 생성 시 필요한 스니펫 파일들이 위치한 디렉토리를 input으로 지정
    inputs.dir snippetsDir

    // source가 없으면 .adoc파일을 전부 html로 만들어버림
    // source 지정시 특정 adoc만 HTML로 만든다.
    sources {
        include("**/index.adoc", "**/common/*.adoc")
    }

    // 소스 파일의 디렉토리 구조를 기반으로 빌드 디렉토리의 기본 디렉토리를 설정합니다.
    baseDirFollowsSourceFile()
}

// Asciidoctor 작업 전 준비 단계
// asciidoctor 작업이 실행되기 전에 기존의 static/docs 디렉토리를 삭제하고, 새로 생성합니다. 이는 이전 문서 파일들이 남아있지 않도록 보장합니다
asciidoctor.doFirst {
    delete file('src/main/resources/static/docs')
    file('src/main/resources/static/docs').mkdirs()
}

// asciidoctor 작업 이후 생성된 HTML 파일을 static/docs로 copy
tasks.register('copyDocument', Copy) {
    dependsOn asciidoctor // copyDocument 작업은 asciidoctor 작업이 완료된 후 실행됩니다.
    from file("build/docs/asciidoc/index.html")
    into file("src/main/resources/static/docs")

}

// build 작업이 실행될 때 copyDocument 작업도 함께 실행되도록 설정합니다.
// 빌드 과정에서 항상 최신 문서가 포함되도록 보장함.
tasks.named('build') {
    dependsOn copyDocument
}

// bootJar 작업이 copyDocument 작업에 의존하도록 설정하여, JAR 파일 생성 시 최신 문서가 포함되도록 합니다.
bootJar {
    dependsOn copyDocument
    from("build/docs/asciidoc/index.html") {
        into 'static/docs'
    }
}



```

### **API 문서 포맷 만들기**

프로젝트의 src/docs/ascidoc위치에 index.adoc 파일을 생성하고 API 문서 포맷을 작성합니다.



```tex
= REST Docs
academy-platform
:doctype: book
:icons: font
:source-highlighter: highlightjs // 문서에 표기되는 코드들의 하이라이팅을 highlightjs를 사용
:toc: left // toc (Table Of Contents)를 문서의 좌측에 두기
:toclevels: 2
:sectlinks:

== Introduction

This document describes the academy_platform API endpoints.

[[User-API]]
== User API

[[User-회원가입]]
== 회원가입

=== 성공적인 회원 가입

operation::join_user[snippets='http-request,request-fields,http-response,response-fields,response-body']

=== 중복된 아이디로 가입 실패

operation::join_user_fail_duplicate_id[snippets='http-request,request-fields,http-response']

[[User-로그인]]
== User 로그인

'''
=== 로그인 성공

operation::login_user[snippets='form-parameters,http-request,http-response,response-fields']
'''

=== 잘못된 아이디 입력

operation::login_user_failed_id[snippets='form-parameters,http-request,response-fields,http-response']
'''

=== 잘못된 비밀번호 입력

operation::login_user_failed_password[snippets='form-parameters,http-request,response-fields,http-response']

```

![image](https://github.com/user-attachments/assets/01f48272-cdd8-40f9-afa7-92bcefee962a)
