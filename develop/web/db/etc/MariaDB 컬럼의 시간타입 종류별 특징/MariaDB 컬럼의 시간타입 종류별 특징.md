# MariaDB 컬럼의 시간타입 종류별 특징

- MariaDB에서 시간 관련 데이터 타입 중 `TIMESTAMP`와 다른 시간 관련 데이터 타입(`DATETIME`, `TIME` 등)의 주요 차이를 정리
- `TIMESTAMP`는 시간대 변환이 필요한 경우 유용하며, `DATETIME`은 시간대를 고려하지 않는 데이터를 다룰 때 적합하다.

---

# TIMESTAMP

## **저장되는 시간**

- UTC 기준 시간으로 저장되며, 조회 시 서버 또는 세션 타임존에 맞춰 변환됨.
- 시간대(Timezone)에 의존적임.

## **범위**

- 1970-01-01 00:00:01 UTC ~ 2038-01-19 03:14:07 UTC

## **자동 설정 기능**

- `DEFAULT CURRENT_TIMESTAMP`, `ON UPDATE CURRENT_TIMESTAMP`와 같은 자동 설정 옵션 제공.
- INSERT나 UPDATE 시 자동으로 현재 시간이 저장되도록 설정 가능.

## **공간 효율성**

- 4바이트로 저장되어 더 적은 스토리지를 사용.

---

# DATETIME

## **저장되는 시간**

- 서버 또는 세션 타임존과 무관하게, 입력된 값 그대로 저장됨.
- 시간대(Timezone)에 비의존적임.

## **범위**

- 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59

## **자동 설정 기능**

- 자동으로 현재 시간을 저장하는 기능 없음(`DEFAULT CURRENT_TIMESTAMP`는 `TIMESTAMP` 전용).

## **공간 효율성**

- 8바이트로 저장되어 더 많은 스토리지를 사용.

---

# TIME

## **저장되는 시간**

- 특정 시간(시, 분, 초)을 저장. 날짜는 포함하지 않음.

## **범위**

- 838:59:59 ~ 838:59:59

## **사용 사례**

- 지속 시간(duration) 또는 하루 시간 표현에 적합.

---

# 주요 차이점 요약

## **시간대 의존 여부**

- `TIMESTAMP`: 시간대에 따라 변환
- `DATETIME`: 변환 없음
- `TIME`: 시간 정보만 저장

## **범위**

- `TIMESTAMP`: 1970-2038년
- `DATETIME`: 1000-9999년
- `TIME`: -838:59:59 ~ 838:59:59

## **자동화 기능**

- `TIMESTAMP`는 자동 설정 지원, `DATETIME`은 지원하지 않음.

## **스토리지**

- `TIMESTAMP`: 4바이트
- `DATETIME`: 8바이트
- `TIME`: 3바이트