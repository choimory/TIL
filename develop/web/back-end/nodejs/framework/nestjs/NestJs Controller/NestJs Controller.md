# 요청값 설정

| `@Request(), @Req()` | `req` |
| --- | --- |
| `@Response(), @Res()`***** | `res` |
| `@Next()` | `next` |
| `@Session()` | `req.session` |
| `@Param(key?: string)` | `req.params` / `req.params[key]` |
| `@Body(key?: string)` | `req.body` / `req.body[key]` |
| `@Query(key?: string)` | `req.query` / `req.query[key]` |
| `@Headers(name?: string)` | `req.headers` / `req.headers[name]` |
| `@Ip()` | `req.ip` |
| `@HostParam()` | `req.hosts` |

# Request parameter

```tsx
@Get()
findAll(@Query() query: ListAllEntities) {
  return `This action returns all cats (limit: ${query.limit} items)`;
}
```

# Request payload

```tsx

export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}

---

@Post()
async create(@Body() createCatDto: CreateCatDto) {
  return 'This action adds a new cat';
}
```

# Path parameter

```tsx
@Get(':id')
async find(@Param('id', new ParseUUIDPipe()) id: string) {
  return this.memberService.find(id);
}
```

# 별도의 응답코드 설정

```tsx
@Post()
@HttpCode(HttpStatus.CREATED)
async join(@Body() payload: JoinMemberRequestDto) {
return this.memberService.join(payload);
}

---

@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

# 와일드카드 Path 설정

```tsx

@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

# 응답 헤더 설정

```tsx
@Post()
@Header('Cache-Control', 'no-store')
create() {
  return 'This action adds a new cat';
}
```

# 리다이렉션 설정

```tsx
@Get()
@Redirect('https://nestjs.com', 301)

---

@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
  if (version && version === '5') {
    return { url: 'https://docs.nestjs.com/v5/' };
  }
}
```

# 참고

- https://docs.nestjs.com/controllers