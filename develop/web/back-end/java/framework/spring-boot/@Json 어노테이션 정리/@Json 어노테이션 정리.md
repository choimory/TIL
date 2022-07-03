# 요약

# @JsonIgnore, @JsonIgnoreProperties, @JsonIgnoreType

- `@JsonIgnore`: Json으로 직렬화할때 해당 어노테이션이 붙은 필드는 직렬화에서 제외한다
- `@JsonIgnoreProperties`: 필드마다 `@JsonIgnore`를 부여하는게 번거롭거나 보기 좋지 않을시, 클래스에 `@JsonIgnoreProperties`를 부여해 여러 필드를 한번에 적용할 수 있다
- `@JsonIgnoreType`: 클레스 전체를 직렬화에서 제외할시 해당 클래스에 `@JsonIgnoreType` 어노테이션을 부여한다
- 서로 참조중인 양방향관계에서 Json 직렬화를 시도할시, 순환참조로 인해 스택오버플로우가 발생하므로 이때 Json 직렬화시엔 양방향이 아닌 단방향으로 만들기 위해 많이 사용되는 어노테이션들이다.
- [https://velog.io/@hth9876/JsonIgnorePropertiesignoreUnknown-true](https://velog.io/@hth9876/JsonIgnorePropertiesignoreUnknown-true)

```java
@Builder
@RequiredArgsConstructor
@Getter
public class MemberDto {
  private final Long id;
  private final String idName;
  private final String nickname;
  private final String email;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime deletedAt;
  private final MemberAuthorityDto memberAuthority;
  private final List<MemberSocialDto> memberSocials;
  private final List<MemberSuspensionDto> memberSuspensions;
}

@Builder
@RequiredArgsConstructor
@Getter
public static class MemberAuthorityDto{
  @JsonIgnore
  private final MemberDto memberDto;
  private final AuthLevel authLevel;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime deletedAt;
```

- MemberDto와 MemberAuthority가 서로를 참조할때 직렬화하면 순환참조가 발생하여 문제가 생긴다. 
  - 그래서 이때 MemberDto와 MemberAuthorityDto 중 한곳에 @JsonIgnore를 걸어, Json직렬화 대상에서 제외함으로 순환참조를 막을수 있다. 
  - 하지만 사실 DTO 객체는 그냥 필드 자체를 선언하지 않도록 설계하는게 더 나은 선택이라고 생각하며, 엔티티의 경우는 필드 선언은 해야하겠지만, 다만 Json 직렬화에 사용되지 않도록 하지 않는게 좋다고 생각.
  - 이런점을 고려한 코딩을 한다면 보통 순환참조보다는, 외부에 응답하지 않아야 할 필드를 처리하는 용도로 더 많이 쓰게 된다. 

```java
@Builder
@RequiredArgsConstructor
@Getter
@JsonIgnoreProperties(value = {"memberDto", "authLevel"})
public static class MemberAuthorityDto {
  private final MemberDto memberDto;
  private final AuthLevel authLevel;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime deletedAt;
}
```

- 사용이유는 @JsonIgnore와 같으며, 단 @JsonIgnore를 여러개 붙이는게 지저분할 경우에 해당 어노테이션으로 대체하여 깔끔하게 사용 가능하다.

```java
```

# @JsonUnwrapped

```java
@Builder
@RequiredArgsConstructor
@Getter
public class MemberDto {
  private final Long id;
  private final String idName;
  private final String nickname;
  private final String email;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime deletedAt;
  @JsonUnwrapped
  private final MemberAuthorityDto memberAuthority;
  private final List<MemberSocialDto> memberSocials;
  private final List<MemberSuspensionDto> memberSuspensions;
}


@Builder
@RequiredArgsConstructor
@Getter
public static class MemberAuthorityDto {
  private final AuthLevel authLevel;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private final LocalDateTime deletedAt;
}
```

```json
//MemberAuthority @JsonUnwrapped 전
{
  "status": 200,
  "message": "OK",
  "member": {
    "id": 1,
    "idName": "choimory",
    "nickname": "중윤최",
    "email": "choimory@naver.com",
    "createdAt": "2000-01-01 00:00:00",
    "modifiedAt": "2000-01-01 00:00:00",
    "deletedAt": null,
    "memberAuthority": {
      "authLevel": "MEMBER",
      "createdAt": "2000-01-01 00:00:00",
      "modifiedAt": "2000-01-01 00:00:00",
      "deletedAt": null
    },
    "memberSocials": [],
    "memberSuspensions": []
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/member/choimory"
    }
  }
}

//MemberAuthority @JsonUnwrapped 후
{
  "status": 200,
  "message": "OK",
  "member": {
    "id": 1,
    "idName": "choimory",
    "nickname": "중윤최",
    "email": "choimory@naver.com",
    "createdAt": "2000-01-01 00:00:00",
    "modifiedAt": "2000-01-01 00:00:00",
    "deletedAt": null,
    "authLevel": "MEMBER",
    "createdAt": "2000-01-01 00:00:00",
    "modifiedAt": "2000-01-01 00:00:00",
    "deletedAt": null,
    "memberSocials": [],
    "memberSuspensions": []
  },
  "_links": {
    "self": {
      "href": "http://localhost:8080/member/choimory"
    }
  }
}
```

- Json으로 직렬화할때 해당 객체 내의 필드를 객체>필드 형태로 직렬화 하지 않고, 객체에서 필드를 꺼내 직렬화 한다

# @JsonInclude

```java
@Getter
@JsonInclude(value = JsonInclude.Include.ALWAYS)
public class MemberViewResponse extends RepresentationModel<MemberViewResponse> {
    private final int status;
    private final String message;
    @JsonInclude(value = JsonInclude.Include.ALWAYS)
    private final MemberDto member;

```

- 해당 어노테이션이 붙은 필드를 값이 없어도 Json 필드에 항시 포함시할지 여부
- 클래스와 필드에 적용 가능하며 클래스에는 모든 필드가 적용되고, 필드에 붙일시 해당 필드에만 적용된다.
- 기본값은 ALWAYS이며 그 외의 값은 다음과 같음.
    - `ALWAYS`: 항시 포함
    - `NON_NULL`: 널은 제외한다 
    - `NON_ABSENT`: NON_NULL에 Optional, AtomicReference의 Absent값이 포함 
    - `NON_EMPTY`: 널, 값이 빈 경우는 제외한다
    - `NON_DEFAULT`: 널과 primitive 기본값인 경우 제외한다 (int 0, boolean false..)
    - `CUSTOM`: valueFilter와 contentFilter를 커스텀하여 사용
- 그 외 자세한 사항은 JsonInclude enum의 문서를 참고

# @JsonProperty, @JsonNaming

```java
@Builder
@RequiredArgsConstructor
@Getter
public class MemberDto {
  @JsonProperty(value = "json_id_field_name")
  private final Long id;
  private final String idName;
  private final String nickname;
  private final String email;
}
```

- Json을 필드로 역직렬화할때 해당 어노테이션이 붙은 필드는 해당 어노테이션에 붙은 이름으로 필드명과 매칭되어 바인딩된다
- e.g 데이터 수집을 위한 API 호출결과가 JSON 구조의 String일때 필드명이 A인데, 객체로 변환할 클래스의 필드명은 one이라면?
  - `@JsonProperty("A")`를 one이라는 필드에 붙여주면, objectMapper등 Jackson이, Json 역직렬화 할때 매핑되어 바인딩 된다.

```java
@Builder
@RequiredArgsConstructor
@Getter
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class MemberDto {
  private final Long id;
  private final String idName;
  private final String nickname;
  private final String email;
}
```  

- 필드명은 같은데 케이스만 다를땐, @JsonNaming을 활용해 모든 필드를 한번에 처리할 수 있다
- [https://cbw1030.tistory.com/315](https://cbw1030.tistory.com/315)

# @JsonFormat

```java
@Builder
@RequiredArgsConstructor
@Getter
public class MemberDto {
    private final Long id;
    private final String idName;
    private final String nickname;
    private final String email;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private final LocalDateTime createdAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private final LocalDateTime modifiedAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private final LocalDateTime deletedAt;
```

- 어떤 형태로 직렬화/역직렬화 할것인지. 날짜관련 필드에 많이 사용된다.
- Json으로 요청, 응답할때 형식을 지정해 변환할때 주로 사용된다.



