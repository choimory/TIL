# Nest.js module

- Nest.js에서의 모듈은 각각의 도메인을 하나의 모듈단위로 묶어 도메인간의 import, export를 관리하여 유용하게 재사용하도록 제공 및 관리한다
- Nest.js의 모듈(module)은 애플리케이션을 구성하는 기본적인 빌딩 블록이다.
- 모듈은 관련된 기능을 하나의 단위로 캡슐화하며, 이를 통해 애플리케이션을 논리적으로 나누고 관리하기 쉽게 만들어 준다.
- Nest.js 애플리케이션은 최소한 하나의 루트 모듈을 가지고 있으며, 여러 하위 모듈로 구성될 수 있다.

---

# 모듈의 개념

## **독립적인 단위**:

모듈은 특정 기능이나 도메인과 관련된 서비스, 컨트롤러, 프로바이더를 그룹화한다.

## **재사용성**:

모듈은 다른 애플리케이션에서도 재사용할 수 있도록 설계될 수 있다.

## **구조화된 설계**:

모듈 기반 설계를 통해 코드베이스를 분리하여 유지보수를 용이하게 한다.

---

# 모듈의 정의

Nest.js의 모듈은 기본적으로 데코레이터를 사용하여 정의된다.

```tsx
import { Module } from '@nestjs/common';

@Module({
  imports: [], // 다른 모듈을 가져오는 배열
  controllers: [], // 이 모듈 내의 컨트롤러 배열
  providers: [], // 서비스나 기타 프로바이더 배열
  exports: [], // 외부에서 사용할 수 있도록 내보낼 프로바이더 배열
})
export class AppModule {}
```

---

# 주요 구성 요소

## **imports**:

- 이 모듈에서 사용할 다른 모듈을 가져온다.
- 순환 종속성을 해결하거나 기능을 재사용하기 위해 사용된다.

## **controllers**:

- 이 모듈과 관련된 HTTP 요청을 처리하는 컨트롤러를 정의한다.

## **providers**:

- 이 모듈 내에서 사용될 서비스, 헬퍼 함수, 또는 기타 종속성을 주입할 객체를 정의한다.
- `@Injectable()` 데코레이터를 사용하여 의존성 주입 가능하게 설정.

## **exports**:

- 이 모듈의 프로바이더를 외부 모듈에서 사용할 수 있도록 내보낸다.

---

# 모듈 간 의존성

모듈은 `imports`와 `exports`를 통해 서로 의존성을 관리한다.

- **import 예시**:

    ```tsx
    import { Module } from '@nestjs/common';
    import { UserModule } from './user/user.module';
    
    @Module({
      imports: [UserModule],
    })
    export class AppModule {}
    ```

- **exports 예시**:

    ```tsx
    import { Module } from '@nestjs/common';
    import { UserService } from './user.service';
    
    @Module({
      providers: [UserService],
      exports: [UserService],
    })
    export class UserModule {}
    ```


---

# 글로벌 모듈

Nest.js에서는 특정 모듈을 글로벌로 설정할 수 있다.

글로벌 모듈은 한 번만 등록하면 애플리케이션의 모든 곳에서 사용할 수 있다.

```tsx
import { Module, Global } from '@nestjs/common';

@Global()
@Module({
  providers: [CommonService],
  exports: [CommonService],
})
export class CommonModule {}
```

- 글로벌 모듈은 `@Global()` 데코레이터를 사용하며, `imports` 없이도 모든 모듈에서 접근 가능하다.

---

# 모듈 설계 패턴

## **도메인 중심 설계**:

- 각 모듈은 특정 도메인과 관련된 기능을 담당한다.
- 예: `UserModule`, `ProductModule`, `OrderModule`.

## **기능 기반 설계**:

- 공통적으로 사용되는 기능을 모듈로 나눈다.
- 예: `AuthModule`, `LoggingModule`.

## **모듈 분할**:

- 하나의 모듈이 너무 커질 경우, 서브 모듈로 분리하여 관리.

---

# 요약

Nest.js 모듈은 애플리케이션의 논리적 구조를 분리하고 유지보수를 쉽게 하기 위한 기본 단위이다.

이를 활용하면 기능의 재사용성을 높이고 의존성을 명확하게 관리할 수 있다.

- 모듈 = 도메인
- 도메인 및 그 하위의 모든 요소들
- 도메인 단위의 요소들을 모듈로 관리함
    - MemberModule의 @Module에서
      Controller: [...],
      Provider: […],
      Import: […]에 등록된 모든 요소들이 MemberModule의 요소들
- 다른 도메인의 특정 요소를 사용하고 싶을때, 모듈에서 해당 요소 혹은 모듈 전체를 import하여 사용
- 해당 도메인의 요소를 다른 도메인에서 사용케 할때는 export하여 개방해줌

# 참고

- https://docs.nestjs.com/modules