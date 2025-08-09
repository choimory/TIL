# Entity의 생명주기(Transient, Persisntent, Detached, Removed)

- JPA에서 **엔티티의 생명주기**는 엔티티가 JPA의 영속성 컨텍스트(Persistence Context와 상호작용하는 상태
- 크게 **Transient(비영속)**, **Persistent(영속)**, **Detached(준영속)**, **Removed(삭제)** 네 가지 상태로 구분됨.
- 이 생명주기를 이해하는 것은 JPA의 동작 원리와 데이터베이스 연동을 효율적으로 관리하는 데 중요한 개념.

---

# 종류

## **Transient**:

- 단순히 메모리 상에 존재하는 객체로, 영속성 컨텍스트와 관련 없음.

## **Persistent**:

- 영속성 컨텍스트에 의해 관리되며, 변경 사항이 자동으로 데이터베이스에 반영됨.

## **Detached**:

- 한때 영속 상태였으나, 더 이상 영속성 컨텍스트에서 관리되지 않으며, 변경 사항이 반영되지 않음.

## **Removed**:

- 데이터베이스에서 삭제될 예정인 상태.