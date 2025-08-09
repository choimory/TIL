# 기본 Pipes

```tsx
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

- 하위 Pipe들은 @nestjs/common 포함이므로 바로 사용할 수 있음
    - `ValidationPipe`
    - `ParseIntPipe`
    - `ParseFloatPipe`
    - `ParseBoolPipe`
    - `ParseArrayPipe`
    - `ParseUUIDPipe`
    - `ParseEnumPipe`
    - `DefaultValuePipe`
    - `ParseFilePipe`

# 커스텀 Pipe

```tsx

import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}

```

- 모든 파이프는 PipeTransform의 transform()을 구현해야함
- value는 메소드 인수 값이고, metadata는 해당 인수의 메타데이터임
- Validation Pipe는 nestjs에서 기본 제공하므로, Validation Pipe 쓸거면 구현하지 않아도 괜찮다

# zod로 객체 validation하기

```tsx
$ npm install --save zod
```

```tsx

import { PipeTransform, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { ZodSchema  } from 'zod';

export class ZodValidationPipe implements PipeTransform {
  constructor(private schema: ZodSchema) {}

  transform(value: unknown, metadata: ArgumentMetadata) {
    try {
      const parsedValue = this.schema.parse(value);
      return parsedValue;
    } catch (error) {
      throw new BadRequestException('Validation failed');
    }
  }
}

```

```tsx

import { z } from 'zod';

export const createCatSchema = z
  .object({
    name: z.string(),
    age: z.number(),
    breed: z.string(),
  })
  .required();

export type CreateCatDto = z.infer<typeof createCatSchema>;

---

@Post()
@UsePipes(new ZodValidationPipe(createCatSchema))
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}

```

# class-validator로 객체 validation하기

```tsx
$ npm i --save class-validator class-transformer
```

- class-validator는 객체에 @IsEmail같은 데코레이터를 제공하는 패키지이다
- class-transformer는 JSON 같은 리터럴 객체와 클래스의 인스턴스간의 변환을 지원, 직렬화 및 역직렬화 해준다

```tsx

import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { validate } from 'class-validator';
import { plainToInstance } from 'class-transformer';

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) {
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    const object = plainToInstance(metatype, value);
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException('Validation failed');
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}

```

- 작성할 필요는 없다

```tsx

import { IsString, IsInt } from 'class-validator';

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}

---

@Post()
async create(
  @Body(new ValidationPipe({transform:true})) createCatDto: CreateCatDto,
) {
  this.catsService.create(createCatDto);
}

```

- transform 옵션은 class-transformer를 이용해 객체 리터럴의 요청갑을 객체로 변환하는걸 true한다는것

## class-validator 데코레이터 종류

| @IsEmpty() | 값이 null, undefined, "" 인지 확인 |
| --- | --- |
| @IsNotEmpty() | 값이 null, undefined, "" 이 아닌지 확인 |
| @IsIn(values: any[]) | 값이 values 배열에 있는 값인지 확인 |
| @IsNotIn(values: any[]) | 값이 values 배열에 없는 값인지 확인 |
| @IsBoolean() | 값이 boolean 인지 확인 |
| @IsDate() | 값이 Date 인지 확인 |
| @IsString() | 값이 문자열인지 확인 |
| @IsNumber(options: IsNumberOptions) | 값이 number인지 확인 |
| @IsInt() | 값이 Interger인지 확인 |
| @IsArray() | 값이 배열인지 확인 |
| @Min(min: number) | 값이 min 값보다 크거나 같은지 확인 |
| @Max(max: number) | 값이 max 값보다 작거나 같은지 확인 |
| @Contains(seed: string) | 문자열에 매개변수로 주어진 seed 값이 포함인지 확인 |
| @NotContains(seed: string) | 문자열에 매개변수로 주어진 seed 값이 미포함인지 확인 |
| @IsAlpha() | 문자열이 a-zA-Z로만 되어 있는지 확인 |
| @IsAlphanumeric() | 문자열이 숫자나 알파멧인지 확인 |
| @IsAscii() | 문자열이 ASCII 문자인지 확인 |
| @IsEmail(options?: IsEmailOptions) | 문자열이 이메일인지 확인 |
| @IsFQDN(options?: IsFQDNOptions) | 문자열이 도메인 주소인지 확인(예: domain.com) |
| @IsIP(version?: "4"|"6") | 문자열이 IP 주소인지 확인(버전 4 또는 6) |
| @IsJSON() | 문자열이 유효한 JSON 인지 확인 |
| @IsObject() | 문자열이 객체인지 확인(null, 함수, 배열은 false 반환) |
| @Length(min: number, max?: number) | 문자열 길이의 최대 최솟값 확인 |
| @MinLength(min: number) | 문자열의 최소 길이 확인 |
| @MaxLength(max: number) | 문자열의 최대 길이 확인 |
| @Matches(pattern: RegExp, modifiers?: string) | 문자열이 정규표현식에 매치되는 값인지 확인 |
| @ArrayNotEmtpy() | 배열이 빈 배열이 아닌지 확인 |
| @ArrayUnique(identifier?: (o) => any) | 배열에 있는 모든 값들이 고유한 값인지 확인. 참조를 통해 비교 |

# 참고

- https://docs.nestjs.com/pipes
- https://docs.nestjs.com/techniques/validation