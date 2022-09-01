# 목차

- [Java SE와 EE의 차이](#-java-se와-ee의-차이)
- [Java와 CPP의 차이](# Java와 CPP의 차이)
- [Java의 장단점](# Java의 장단점)
- [Java의 접근 제어자 종류](# Java의 접근 제어자 종류)
- [static](# static)
- [Java 데이터 타입](# Java 데이터 타입)
- [Wrapper class](# Wrapper class)
- [OOP 특징](# OOP 특징)
- [OOP 5대 원칙 (SOLID)](# OOP 5대 원칙 (SOLID))
- [객체지향과 절차지향의 차이](# 객체지향과 절차지향의 차이)
- [Java 인스턴스 필드와 static 필드의 차이](# Java 인스턴스 필드와 static 필드의 차이)
- [Java 메인 메소드가 static인 이유](# Java 메인 메소드가 static인 이유)
- [Java의 static 메모리 활용 방식](# Java의 static 메모리 활용 방식)
- [Java의 final, finally, finalize](# Java의 final, finally, finalize)
- [직렬화와 역직렬화](# 직렬화와 역직렬화)
- [클래스, 객체, 인스턴스의 차이](# 클래스, 객체, 인스턴스의 차이)
- [오버로딩과 오버라이딩의 차이](# 오버로딩과 오버라이딩의 차이)
- [Call by Ref와 Call by Value의 차이](# Call by Ref와 Call by Value의 차이)
- [인터페이스와 추상 클래스의 차이](# 인터페이스와 추상 클래스의 차이)
- [JVM 구조](# JVM 구조)
- [String, StringBuffer, StringBuilder](# String, StringBuffer, StringBuilder)
- [동기화와 비동기화의 차이](# 동기화와 비동기화의 차이)
- [Java에서 ==와 equals()의 차이](# Java에서 ==와 equals()의 차이)
- [Java의 리플렉션](# Java의 리플렉션)
- [Lambda](# Lambda)
- [Stream](# Stream)
- [동기화, 비동기화](# 동기화, 비동기화)
- [Annotation](# Annotation)
- [GC 처리 방법](# GC 처리 방법)
- [제네릭스](#-제네릭스)

---

# Java SE와 EE의 차이

- SE: Standard Edition, 기본적인 JDK만 포함
- EE: Enterprise Edition, 모든 JDK 포함

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

- S:
- O:
- L:
- I:
- D:

---

# 객체지향과 절차지향의 차이

---

# Java 인스턴스 필드와 static 필드의 차이

- 인스턴스 필드
  - 인스턴스마다 값이 변해야 되는 경우
  - 스프링 컨텍스트로부터 인스턴스를 주입받아야 하는 경우
- static 핃르
  - 모든 인스턴스가 같은 값을 고정적으로 사용하는 경우
  - 주입받을 필요 없이 사전부터 초기화되어 사용되는 경우

---

# Java 메인 메소드가 static인 이유

---

# Java의 static 메모리 활용 방식

- JVM Method Area에 static 하나만 올려놓고 범용적으로 사용

---

# Java의 final, finally, finalize

- final: 초기화 이후 값이 변해지는것을 금지
- finally: try catch 이후 공통적으로 수행되어야 하는 로직을 수행
- finalize: 특정 인스턴스의 종료 메소드

---

# 직렬화와 역직렬화

---

# 클래스, 객체, 인스턴스의 차이

---

# 오버로딩과 오버라이딩의 차이

- 오버로딩: 같은 메소드를 다른 파라미터로 여러개 만드는것
- 오버라이딩: 상속받은 메소드를 재정의하는것

---

# Call by Ref와 Call by Value의 차이

- 

---

# 인터페이스와 추상 클래스의 차이

- 인터페이스는 상수와 public 메소드 선언, default 메소드 작성만 가능하다
- 추상 클래스는 일반 클래스와 비슷하게 작성이 가능하나, 구현이 필요한 메소드가 하나라도 있을경우 추상클래스 선언이 가능하다
- 고로 인터페이스가 더 제한적이며, 더 많은 구현을 필요로 하므로 미리 구현해야 할 메소드나 필드가 있을 경우 추상클래스를, 그렇지 않은 경우 인터페이스 사용을 고려 할 수 있다.

---

# JVM 구조

## Method Area

- 클래스 메타 데이터, static 데이터 등이 올라간다

## Stack Area

- 메소드 실행 정보 및 지역변수 등이 올라간다
- Stack 형태로 되어 있어, FILO이다.

## Heap Area

- 인스턴스 생성 정보가 올라간다
- 주소 형태로 관리되며, 모든 참조형 변수는 Heap Area의 주소가 저장된다
- 참조되지 않는 인스턴스는 GC에 의해 정리된다

---

# String, StringBuffer, StringBuilder

## String

## StringBuffer

## StringBuilder

---

# 동기화와 비동기화의 차이

- async
- synchronized

---

# Java에서 ==와 equals()의 차이

---

# Java의 리플렉션

---

# Lambda

---

# Stream

---

# Annotation

---

# GC 처리 방법

---

# 제네릭스

---