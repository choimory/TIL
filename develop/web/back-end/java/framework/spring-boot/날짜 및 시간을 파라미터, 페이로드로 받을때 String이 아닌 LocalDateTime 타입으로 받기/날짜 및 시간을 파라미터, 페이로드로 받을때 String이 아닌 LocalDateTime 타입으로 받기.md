# 요약

- Parameter로 받을땐 @DateTimeFormat - 스프링 지원 형변환
- Json Payload로 받을땐 @JsonFormat - Jackson 라이브러리 지원 Json 직렬화
- @JsonFormat으로 pattern을 변경해서 받을땐 불변을 포기해야한다
- 기본 형식인 yyyy-MM-ddTHH:mm:ss 같은 일자와 시간 사이에 T가 들어가는 방식은 권장사항이며 굳이 바꿀 필요는 없다

# @DateTimeFormat - Parameter로 받을시

```java
// 컨트롤러

@GetMapping
public ResponseEntity<MemberListResponse> views(final MemberListRequest param,
                                                final CommonPageRequest commonPageRequest){
    return new ResponseEntity<>(memberService.views(param, commonPageRequest.of(MemberDefaultSort.VIEWS.getProperty(), MemberDefaultSort.VIEWS.getDirection())),
            HttpStatus.OK);
}
```

```java
// 요청필드 객체

@Builder
@RequiredArgsConstructor
@Getter
public class MemberListRequest {
    private final String idName;
    private final String nickname;
    private final String email;
    private final AuthLevel authLevel;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime createdFrom;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime createdTo;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime modifiedFrom;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime modifiedTo;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime deletedFrom;
    @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME, pattern = "yyyy-MM-dd HH:mm:ss")
    private final LocalDateTime deletedTo;
}
```

- RequestParemeter로 받을 필드엔 @DateTimeFormat 어노테이션을 사용하면 LocalDateTime으로 요청값을 받을수 있다
- pattern에는 클라이언트가 보낼 요청형식을 입력한다

# @JsonFormat - Payload로 받을시


```java
// 컨트롤러

@PostMapping
public ResponseEntity<MemberListResponse> views(@RequestBody(required=false) final MemberListRequest param,
                                                final CommonPageRequest commonPageRequest){
    return new ResponseEntity<>(memberService.views(param, commonPageRequest.of(MemberDefaultSort.VIEWS.getProperty(), MemberDefaultSort.VIEWS.getDirection())),
            HttpStatus.OK);
}
```

```java
// 요청필드 객체

@Builder
@RequiredArgsConstructor
@Getter
public class MemberListRequest {
    private final String idName;
    private final String nickname;
    private final String email;
    private final AuthLevel authLevel;
    private final LocalDateTime createdFrom;
    private final LocalDateTime createdTo;
    private final LocalDateTime modifiedFrom;
    private final LocalDateTime modifiedTo;
    private final LocalDateTime deletedFrom;
    private final LocalDateTime deletedTo;
}
```

- 일단 json으로 받을때는 클라이언트가 "yyyy-MM-ddTHH:mm:ss" 형식으로 보낸다면 임의의 설정 필요없이 자동적으로 형변환된다.

```java
// final과 필수생성자를 죽여 불변을 포기하고, setter를 사용하도록 한 상태

// 컨트롤러
@PostMapping
public ResponseEntity<MemberListResponse> views(@RequestBody(required = false) final MemberListRequest param,
                                                                                final CommonPageRequest commonPageRequest){
    return new ResponseEntity<>(memberService.views(param, commonPageRequest.of(MemberDefaultSort.VIEWS.getProperty(), MemberDefaultSort.VIEWS.getDirection())),
            HttpStatus.OK);
    }

// 요청객체
@Builder
@Setter
@Getter
@NoArgsConstructor
@AllArgsConstructor
public class MemberListRequest {
    private String idName;
    private String nickname;
    private String email;
    private AuthLevel authLevel;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime createdFrom;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime createdTo;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime modifiedFrom;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime modifiedTo;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime deletedFrom;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyyMMdd HHmmss")
    private LocalDateTime deletedTo;
}
```  

- @JsonFormat의 pattern을 이용해 원하는 형태로 받을때에는 필수생성자+final이 아닌, 기본생성자+setter 방식일때만 적용된다
- Json deserialize에는 Jackson 라이브러리가 사용되는데, 해당 라이브러리의 직렬화 방식에 해당 메소드들이 요구되기 때문에 @JsonFormat을 이용해 원하는 방식으로 받은 값을 초기화할때는 해당 내용들이 필요한게 아닌가 추측
- 하지만 보통 값을 받을때 "yyyy-MM-ddTHH:mm:ss" 같이 일자와 시간 사이 T를 넣는건 오히려 권장되는 방식이기 때문에, 요청값 불변을 포기하면서 까지 패턴을 변경하여 진행하는것보단 그냥 위의 기본내용으로 진행하는게 좋지 않을까 생각한다.

# 참고

- [https://jojoldu.tistory.com/361](https://jojoldu.tistory.com/361)
- [https://swampwar.github.io/2020/03/19/LocalDateTime-%EB%B3%80%EC%88%98%EB%B0%94%EC%9D%B8%EB%94%A9.html](https://swampwar.github.io/2020/03/19/LocalDateTime-%EB%B3%80%EC%88%98%EB%B0%94%EC%9D%B8%EB%94%A9.html)