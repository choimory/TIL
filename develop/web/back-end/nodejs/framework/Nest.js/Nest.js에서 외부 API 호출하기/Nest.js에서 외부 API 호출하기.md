# 들어가며

- Nest.js는 외부 API와 통신하기 위해 Nest 프레임워크에서 기본 제공하는 HttpModule가 있음
- HttpModule의 HttpService를 사용하여 API 요청하면 됨
- httpService.axiosRef.get...하면 axios의 호출과 동일하게 사용 가능
- https://docs.nestjs.com/techniques/http-module

# HttpModule 주입받기

```typescript
@Module({
  providers: [MyService],
  imports: [HttpModule],
  exports: [MyService],
})
export class MyModule {}
```

- 모듈

```typescript
@Injectable()
export class MyService {
  constructor(readonly httpService: HttpService) {}
    
  ...
```

- 서비스

# 사용하기

```typescript
  public async callApi(): Promise<ResponseDto<ApiData>> {
    return this.httpService.axiosRef.get|post|....("url", {headers:{}, params:{}...})
        .then(..)
        .then(..)
        .catch(..);
  }
```