## JAR vs WAR

JAR와 WAR 모두 JAVA의 `jar` 툴을 이용해 생성된 파일이며 애플리케이션을 쉽게 배포하고 동작시킬 수 있도록 관련 파일을 패키징해준다.

- **JAR (Java Archive)**
    - `.jar` 자바 프로젝트를 압축한 파일
    - 자바 리소스, 속성파일, 라이브버리 등이 포함되어있음
    - **원하는 구조**로 구성이 가능하며 JDK(Java Development Kit)에 포함되어 있는 **JRE(Java Runtime Environment)만 가지고도 실행 가능**하다.
- **WAR(Web Application Archive)**
    - `.war` servlet / jsp 컨테이너에 배치할 수 있는 웹어플리케이션 압축 파일
    - 웹 관련 자원만을 포함 (JSP, Servlet, JAR, Class, HTML 등)
    - JAR과 달리 WEB-INF 및 META-INF 디렉토리로 **사전 정의된 구조**를 사용하며 실행하기 위해서 Tomcat과 같은 **웹 서버 또는 웹 컨테이너(WAS)가 필요**하다.
    - WAR도 JAVA의 jar 옵션(`java -jar`)을 이용해 생성하는 **JAR 파일의 일종**이다.