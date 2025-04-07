# Nest.js Provider

NestJS의 **Provider**는 의존성 주입(Dependency Injection)을 통해 객체를 생성하고 관리하며, 애플리케이션의 구성 요소 간 의존성을 해결하는 핵심 개념이다.

NestJS에서 서비스, 리포지토리, 팩토리, 헬퍼 클래스 등 다양한 비즈니스 로직을 캡슐화한 요소를 **Provider**로 정의할 수 있다.

---

# 주요 특징

## **의존성 관리**:

NestJS의 `Dependency Injection Container`를 통해 필요한 객체를 주입받아 사용할 수 있다.

## **범위(Scope)**:

Providers는 기본적으로 싱글톤(singleton)으로 작동하며, 특정 상황에서 요청 단위(request) 또는 트랜지언트(transient) 범위로 동작하도록 설정할 수 있다.

## **사용처**:

서비스, 유틸리티 클래스, 데이터베이스 접근 계층 등에서 활용된다.

---

# Provider 정의

Provider는 기본적으로 **클래스**, **값**, **팩토리 함수**로 정의될 수 있다.

## **클래스 기반 Provider**

클래스 자체를 Provider로 등록하며, 대부분의 서비스 구현이 이 방식으로 이루어진다.

```tsx
@Injectable()
export class UserService {
    getUser() {
        return 'User data';
    }
}
```

## **값(Value) Provider**

단순한 값을 주입할 때 사용한다.

```tsx
const API_KEY = '123456';

@Module({
    providers: [
        { provide: 'API_KEY', useValue: API_KEY }
    ],
})
export class AppModule {}
```

## **팩토리(Factory) Provider**

동적으로 값을 생성해야 하는 경우 사용한다.

```tsx
@Module({
    providers: [
        {
            provide: 'CONFIG',
            useFactory: () => {
                return { host: 'localhost', port: 3000 };
            }
        }
    ],
})
export class AppModule {}
```

---

# Provider 주입

Provider는 클래스의 생성자에 주입되어 사용된다.

`@Injectable` 데코레이터가 필요하며, NestJS는 자동으로 의존성을 해결한다.

```tsx
@Injectable()
export class AppService {
    constructor(private readonly userService: UserService) {}

    getHello(): string {
        return this.userService.getUser();
    }
}
```

---

# 커스텀 토큰 사용

기본 클래스명 외에 커스텀 토큰을 사용해 Provider를 정의하고 주입할 수도 있다.

```tsx
@Module({
    providers: [
        { provide: 'CUSTOM_TOKEN', useClass: UserService },
    ],
})
export class AppModule {}

// 주입 시
@Injectable()
export class AppService {
    constructor(@Inject('CUSTOM_TOKEN') private readonly userService: UserService) {}
}
```

---

# 요약

Provider는 NestJS의 의존성 주입 시스템의 중심이며, 다양한 방식으로 구성 요소 간 의존성을 해결하는 데 사용된다.

클래스, 값, 팩토리 등을 활용해 유연하게 정의할 수 있으며, 주로 서비스와 같은 비즈니스 로직을 캡슐화하는 데 사용된다.

# 참고

- https://docs.nestjs.com/providers