# Java SE와 EE의 차이

- SE: Standard Edition
- EE: Enterprise Edition

---

# Java와 CPP의 차이

- Java는 실행환경에서 컴파일러가 바로 바이트 코드를 생성하지만, cpp는 컴파일 외에 링크라는 과정이 존재한다

---

# Java의 장단점

- 운영체제에 구애받지 않음
- OOP
- GC에게 메모리관리 위임하여 비즈니스 로직 집중
- 멀티 스레딩
- 동적 로딩
    - 어플리케이션 시작이 아닌, 객체 필요 시점에 특정 클래스 로딩
- 느린 속도
- 예외처리

---

# Java의 접근 제어자 종류

- public: 전체 접근 가능
- protected: 같은 클래스 및 상속 관계만 접근 가능
- default: 같은 패키지간 접근 가능
- private: 클래스 내에서만 접근 가능

---

# static 

- 클래스의 인스턴스 필드나 메소드와 관련이 없는, 독립적인 클래스, 필드 혹은 메소드는 static을 부여할 수 있다.
- static은 인스턴스와 관련이 없으므로 어떤 인스턴스를 생성하던 결과가 똑같다. 
- 즉 메모리에 인스턴스별로 여러개를 올릴 필요가 없다는 뜻이므로, 클래스 로딩시에 JVM 메소드 메모리 영역에 하나만 올라간다.
- 인스턴스와 관련이 없으므로 사용시에도 인스턴스 생성 없이 사용이 가능하다.

---

# Java 데이터 타입

## primitive

- boolean(true, false) < byte(1byte) < char(2byte) < short(2byte) < int(4byte) < float(4byte) < long(8byte) < double(8byte)
- JVM stack 영역에 저장됨

## Reference

- 기본형 제외 전부
- JVM Heap 영역에 저장되며, 아무도 참조 중이지 않을시엔 GC에 의해 destroyed 됨
- 변수에 값이 아닌 heap 영역의 주소가 초기화 됨

---

# Wrapper class

- 기본형의 객체화
- 타입, 제네릭, null 체크 등 여러 필요성에 의해 생김
- 개발시 기본형보다는 래퍼클래스의 사용을 권장
- 객체이므로 new 연산자 등을 이용해 박싱, 언박싱하여 생성해야 하지만, JDK 1.5부터 컴파일러가 오토 박싱, 오토 언박싱을 진행해준다

---

# OOP 특징

---

# OOP 5대 원칙 (SOLID)

---

# 객체지향과 절차지향의 차이

---

# Java 인스턴스 필드와 static 필드의 차이

---

# Java 메인 메소드가 static인 이유

---

# Java의 static 메모리 활용 방식

---

# Java의 final, finally, finalize

---

# 직렬화와 역직렬화

---

# 클래스, 객체, 인스턴스의 차이

---

# 오버로딩과 오버라이딩의 차이

---

# Call by Ref와 Call by Value의 차이

---

# 인터페이스와 추상 클래스의 차이

---

# JVM 구조

---

# String, StringBuffer, StringBuilder

---

# 동기화와 비동기화의 차이

---

# Java에서 ==와 equals()의 차이

---

# Java의 리플렉션

---

# Lambda

---

# Stream

---

# 동기화, 비동기화

---

# Annotation

---

# GC 처리 방법

---

# 제네릭스

---