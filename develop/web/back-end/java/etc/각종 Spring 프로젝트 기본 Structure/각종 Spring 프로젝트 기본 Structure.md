# Maven Legacy

# Gradle Boot

- ๐src
    - ๐main
        - ๐java
            - ๐com
                - ๐๋ฉ์ธ ํ๋ก์ ํธ ํจํค์ง
                    - ๐๊ฐ์ข ํจํค์ง
                    - ๐๋ฉ์ธํ๋ก์ ํธApplication.java (๋ฉ์ธ ์คํ๋ง ์ปจํ์ด๋ XML ์ญํ )
                    - ๐๊ฐ์ขApllication.java (์คํ๋ง ์ปจํ์ด๋ XML ์ญํ , ๋ฉ์ธ Application.java์ ๊ฐ์ depth์ ์์นํ ๊ฒ)
                - ๐๊ทธ ์ธ ๋ชจ๋ ํจํค์ง
                    - ๐๊ฐ์ข ํจํค์ง
                    - ๐๋ชจ๋ํ๋ก์ ํธApplication.java (๋ฉ์ธ ์คํ๋ง ์ปจํ์ด๋ XML ์ญํ )
                    - ๐๊ฐ์ขApllication.java (์คํ๋ง ์ปจํ์ด๋ XML ์ญํ , ๋ฉ์ธ Application.java์ ๊ฐ์ depth์ ์์นํ ๊ฒ)
    - ๐resources
        - ๐[application.properties](http://application.properties) (๊ฐ์ข ๋ถํธ ์ค์  ํ์ผ, web.xml ๋ฐ ์๋ธ๋ฆฟ ์ปจํ์ด๋ ์ญํ )
        - ๐static (์ ์  ๋ฆฌ์์ค)
            - js, css, font, imgs...
        - ๐templates (๋ทฐ ํ์ด์ง)
            - ํด๋ ๋ฐ html ํ์ผ๋ค
        - ๐upload (์๋ก๋๊ธฐ๋ฅ ์ฌ์ฉ์)
            - ํด๋ ๋ฐ ์๋ก๋ ํ์ผ๋ค
        - ๐mybatis (ORM์ฌ์ฉ์)
        - ๐log.properties, log.xml (logger ์ฌ์ฉ์)
    - ๐test
        - ๐java
            - main๊ณผ ๋์ผ
- ๐bin
    - default
    - main
    - test
- JRE System Library
    - ์๋ฐ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.jar
- Project and External Dependencies
    - ์ธ๋ถ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.jar
- ๐gradle
    - ๐wrapper
        - gradle-wrapper.jar
        - gradle-wrapper.properties
- ๐build.gradle (Maven์ pom.xml ์ญํ )
- ๐gradlew
- ๐gradlew.bat
- ๐HELP.md
- ๐settings.gradle
- ๐.gradle
    - ๊ฐ์ข ํด๋
        - ๊ฐ์ข ์ค์ 
- ๐.settings
    - ์ดํด๋ฆฝ์ค ์ค์ ํ์ผ
- ๐.classpath
- ๐.gitignore
- ๐.project

# Maven Boot

- ๐src
    - ๐main
        - ๐java
            - ๐com
                - ๐๋ฉ์ธ ํ๋ก์ ํธ ํจํค์ง
                    - ๐๊ฐ์ข ํจํค์ง
                    - ๐๋ฉ์ธํ๋ก์ ํธApplication.java (๋ฉ์ธ ์คํ๋ง ์ปจํ์ด๋ XML ์ญํ )
                    - ๐๊ฐ์ขApllication.java (์คํ๋ง ์ปจํ์ด๋ XML ์ญํ , ๋ฉ์ธ Application.java์ ๊ฐ์ depth์ ์์นํ ๊ฒ)
                - ๐๊ทธ ์ธ ๋ชจ๋ ํจํค์ง
                    - ๐๊ฐ์ข ํจํค์ง
                    - ๐๋ชจ๋ํ๋ก์ ํธApplication.java (๋ฉ์ธ ์คํ๋ง ์ปจํ์ด๋ XML ์ญํ )
                    - ๐๊ฐ์ขApllication.java (์คํ๋ง ์ปจํ์ด๋ XML ์ญํ , ๋ฉ์ธ Application.java์ ๊ฐ์ depth์ ์์นํ ๊ฒ)
        - ๐resources
            - ๐[application.properties](http://application.properties) (๊ฐ์ข ๋ถํธ ์ค์  ํ์ผ, web.xml ๋ฐ ์๋ธ๋ฆฟ ์ปจํ์ด๋ ์ญํ )
            - ๐static (์ ์  ๋ฆฌ์์ค)
                - js, css, font, imgs...
            - ๐templates (๋ทฐ ํ์ด์ง)
                - ํด๋ ๋ฐ html ํ์ผ๋ค
            - ๐upload (์๋ก๋๊ธฐ๋ฅ ์ฌ์ฉ์)
                - ํด๋ ๋ฐ ์๋ก๋ ํ์ผ๋ค
            - ๐mybatis (ORM์ฌ์ฉ์)
            - ๐log.properties, log.xml (logger ์ฌ์ฉ์)
    - ๐test
        - ๐java
            - main๊ณผ ๋์ผ
    - JRE System Library
        - ์๋ฐ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.jar
    - Maven Dependencies
        - ์ธ๋ถ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.jar
    - ๐target
        - ํ๋ก์ ํธ ๊ฐ๋ฐํ๊ฒฝ ๊ด๋ จ ๊ฐ์ธ์ค์ 
    - ๐HELP.md
    - ๐mvnw
    - ๐mvnw.cmd
    - ๐pom.xml
    - ๐.mvn
        - ๐wrapper
            - maven-wrapper.jar
            - maven-wrapper.properties
            - MavenWrapperDownloader.java
    - ๐.settings
        - ๊ฐ์ข ์ดํด๋ฆฝ์ค ์ค์  ๊ด๋ จ ํ์ผ
    - ๐.classpath
    - ๐.gitignore
    - ๐.project

# ํน์ด์ฌํญ

- legacy๋ src/main, src/resource, src/webapp ์ธ๊ฐ์ ํด๋๋ฅผ ํตํด main์ ์๋ฐ ์์ค์ฝ๋, resources๋ ORM, webapp์ ํ๋ฉด ๊ด๋ จ ํ์ผ๋ค์ ๊ด๋ฆฌํ์๋๋ฐ, ์ด๋ด๊ฒฝ์ฐ deploy ๋์์ ๊ฒฝ์ฐ์๋ง ํ๋ฉด ๊ด๋ จ ์ ์  ๋ฆฌ์์ค๋ฅผ ์ ์์ ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ค.
- boot๋ webapp์ ์ ๊ฑฐํ๊ณ  src/main์ ์๋ฐ ์์ค์ฝ๋๋ฅผ ๊ด๋ฆฌ, src/resources๋ ํ๋ฉด ๊ด๋ จ ํ์ผ๋ค์ ๊ด๋ฆฌํ๋ค. ORM์ JPA๋ฅผ ์ฌ์ฉํ๊ธฐ์ ์ฌ์ฉ๋์ง ์์ง๋ง, ์ฌ์ฉํ  ๊ฒฝ์ฐ ORM ๊ด๋ จ ๋ฆฌ์์ค๋ resources๋ด์์ ๊ด๋ฆฌํ๋ค. main์ ์๋ฐ, resources๋ ๊ทธ ์ธ ๋ชจ๋  ํ์ผ๋ค์ ๊ด๋ฆฌํ๋ค๊ณ  ํ  ์ ์๋ค.
- ๊ฐ๋ฐ์ ๋ง์์ ๋ฐ๋ผ boot์์๋ webapp์ ๋ง๋  ๋ค, ์ค์ ์ ํตํด webapp๋ด์์ ํ๋ฉด๊ด๋ จ ๋ฆฌ์์ค๋ค์ ๊ด๋ฆฌ, ์ฌ์ฉ ํ  ์ ์๋ค. jsp๋ฅผ ์ฌ์ฉํ  ์๋ ์๋ค.