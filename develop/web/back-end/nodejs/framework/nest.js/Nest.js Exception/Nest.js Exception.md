# 개요

- Nest.js에는 기본적으로 견고한 예외 핸들러가 내장되어 있다
- 예외 발생시 500, 잘못된 값은 400 등 일관적이고 납득 가능한 응답을 처리해준다
- 그러므로 원하는 내용으로 커스텀하여 처리하고 싶을때 사용할만한, 커스텀 예외 및 커스텀 예외용 예외 핸들러만 작성해보자

# NestJs 기본 예외, 기본 예외 필터 사용하기

- 기본 예외를 사용할 시, 기본 응답의 프로퍼티 중, statusCode와 message는 항상 포함된다
- 고로 기본 예외를 사용하는 경우, 별도의 응답을 응답할때도 두 프로퍼티는 항상 같은 필드명으로 응답해주는게 좋다

## 서버 오류시

```tsx
{
    "statusCode": 500,
    "message": "Internal server error"
}
```

- 예상치 못한 예외가 발생했을시, 해당 응답이 일관적으로 응답된다

## pipes validation 걸렸을시

```tsx
{
    "statusCode": 400,
    "message": [
        "email must be an email",
        "nickname should not be empty",
        "password should not be empty"
    ],
    "error": "Bad Request"
}
```

## 기본 예외로 던지기

```tsx
async join(payload: JoinMemberRequestDto): Promise<CommonResponseDto> {
    // check duplicate
    const isExist: boolean = await this.memberRepository
      .createQueryBuilder('m')
      .select()
      .where('m.email=:email or m.nickname=:nickname', {
        email: payload.email,
        nickname: payload.nickname,
      })
      .getExists();

    if (isExist) {
      throw new HttpException(
        'duplicate email or nickname',
        HttpStatus.BAD_REQUEST,
      );
    }
	...
}
```

```tsx
{
    "statusCode": 400,
    "message": "duplicate email or nickname"
}
```

- HttpExpception에 대한 예외들이 잘 처리되어있기 때문에 별도의 커스텀 예외가 사실 굳이 필요하진 않음

---

# 커스텀 예외 생성하여 던지기

- 기본 예외 및 핸들러가 잘되어 있어 굳이 싶긴 하다
- 그래도 프로퍼티의 추가, 변경 등의 상세한 처리가 필요할 경우 고려해볼수 있다

## CustomException

```tsx
import { HttpException, HttpExceptionOptions } from '@nestjs/common';

export class CommonException extends HttpException {
  author: string;

  constructor(
    msg: string | Record<string, any>,
    code: number,
    author: string,
    options?: HttpExceptionOptions,
  ) {
    super(msg, code, options);
    this.author = author;
  }
}

```

## CustomExceptionFilter

```tsx
import { ArgumentsHost, Catch, ExceptionFilter } from '@nestjs/common';
import { CommonException } from './common-exception';

@Catch(CommonException)
export class CommonExceptionFilter implements ExceptionFilter {
  catch(commonException: CommonException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    response.status(commonException.getStatus()).json({
      code: commonException.getStatus(), //프로퍼티명 변경
      msg: commonException.message, //프로퍼티명 변경
      timestamp: new Date().toISOString(), //프로퍼티 추가
      path: request.url, //프로퍼티 추가
      author: commonException.author, //프로퍼티 추가
    });
  }
}

```

## 모듈 등록

```tsx
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { MemberModule } from './v1/member/member.module';
import { ConfigModule } from '@nestjs/config';
import { ConfigModuleOptions } from '@nestjs/config/dist/interfaces/config-module-options.interface';
import Joi from 'joi';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TypeOrmModuleOptions } from '@nestjs/typeorm/dist/interfaces/typeorm-options.interface';
import { SnakeNamingStrategy } from 'typeorm-naming-strategies';
import { APP_FILTER } from '@nestjs/core';
import { CommonExceptionFilter } from './v1/common/exception/common-exception.filter';

@Module({
  imports: [생략...],
  controllers: [AppController],
  providers: [
    AppService,
    { provide: APP_FILTER, useClass: CommonExceptionFilter }, //common exception
  ],
})
export class AppModule {}

```

## 사용

```tsx
@Get()
@UseFilters(CommonExceptionFilter) //컨트롤러 엔드포인트에 등록
async findAll(
    @Query() page: CommonPageRequestDto,
    @Query() param: FindAllMemberRequestDto,
  ) {
	  //example
    throw new CommonException('test', HttpStatus.OK, 'author');
}
```

- 컨트롤러 엔드포인트에 `@UseFilters(CommonExceptionFilter)`
- UseFilters 외 기본 필터들도 정상 동작 함

# 참고

- https://docs.nestjs.com/exception-filters