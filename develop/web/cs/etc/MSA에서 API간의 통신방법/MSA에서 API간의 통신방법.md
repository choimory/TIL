# MSA API간 통신방법

- MSA(Microservice Architecture)에서 API 간 통신이 필요한 경우, 서비스 간의 의존성과 호출 방식을 고려해 적절한 통신 방식을 선택해야 함

---

# **HTTP REST 통신 (동기 방식)**

- 가장 일반적인 방식
- 서비스 간에 HTTP 요청을 통해 데이터를 주고받음

```tsx
// item-api 내부 서비스
@Injectable()
export class MemberService {
  constructor(private readonly httpService: HttpService) {}

  async getMemberInfo(memberId: string): Promise<any> {
    const { data } = await this.httpService
      .get(`http://member-api/internal/members/${memberId}`)
      .toPromise()
    return data
  }
}
```

주의사항:

- 호출 서비스(member-api)가 죽으면 item-api도 영향을 받는 **강한 결합**
- **타임아웃, 재시도, 장애 대응 로직** 필요

---

# **비동기 메시지 기반 통신 (Kafka, RabbitMQ 등)**

- 서비스 간 통신을 **이벤트 기반**으로 처리
- item-api가 "회원정보 필요" 이벤트를 발행하면, member-api가 응답 이벤트를 보냄

## 장점:

- **느슨한 결합**, 고가용성
- 처리량이 많거나 실시간 처리가 중요할 때 유리

## 단점:

- 구현 복잡도 증가
- 응답 지연 가능

```
item-api → "회원정보 요청" 메시지 발행
member-api → 해당 메시지 수신 후, "회원정보 응답" 메시지 발행
item-api → 응답 메시지 수신 후 로직 처리
```

---

# **API Gateway + 서비스 간 호출**

- 모든 요청을 API Gateway에서 관리하고 내부적으로 라우팅
- item-api가 직접 member-api를 호출하지 않고, Gateway를 통해 호출

## 장점:

- 보안, 인증, 로깅 등을 중앙 집중화
- 서비스 간 통신 구조 단순화

## 단점:

- 내부 서비스간 직접 호출보다 약간의 오버헤드

---

# **Service Discovery + Internal DNS**

- 서비스 이름 기반으로 통신
- 예: `http://member-api:3000/members/123`
- 쿠버네티스, Docker Compose, Eureka, Consul 등 사용 가능

---

# 요약

| 방식 | 특징 | 사용 시점 |
| --- | --- | --- |
| REST 호출 | 간단, 빠름, 강한 결합 | 서비스 수 적고, 호출 빈도 적을 때 |
| 메시지 큐 | 확장성, 비동기 | 느슨한 결합 필요할 때 |
| API Gateway | 보안, 라우팅 통합 | 서비스가 많을 때 |
| 서비스 디스커버리 | 유연한 주소 할당 | 컨테이너 환경, 오토스케일링 필요할 때 |

---

- 현재 상황이 "item-api에서 member-api의 회원 정보를 가져와야 함"이라면, **동기 REST 호출 + 재시도/타임아웃 처리 + 에러 핸들링** 구조가 기본 선택지
- 복잡하거나 트래픽 많은 시스템이라면 **비동기 메시지 통신 고려** 가능