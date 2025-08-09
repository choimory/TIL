# 프로젝트 생성

## node, nvm, npm

### node.js

- Java가 개발 및 실행에 JDK가 필요하듯이, Nest는 node.js가 필요

### nvm

- 특정 버전의 node를 직접 설치하는 대신, 여러 버전의 노드들을 사용할 수 있게 관리해주는 nvm을 설치하고, nvm으로 원하는 node를 사용한다

### npm

- 의존성 관리해주는 npm으로 Nest.js를 받고 Nest.js 명령어로 프로젝트 및 리소스를 생성하여 개발을 시작함

## nvm 설치, 사용할 node 설정

```bash
# nvm 설치
brew install --cask nvm

# node install
nvm install <version> #e.g. 16, --lts
nvm use <version>

# check
node -v
npm -v
```

- npm은 node와 함께 자동 설치됨
- version엔 버전 숫자 혹은 —lts등을 작성 가능

## nest.js 설치 및 프로젝트 생성

```bash
# nest.js 설치 
npm install -g @nestjs/cli

# nest.js 프로젝트 생성
nest new <project_name> --restrict
```

- restrict 옵션은 typescript의 타입체크 제한의 강도에 대한 설정
- 프로젝트의 tsconfig.json에서 바꿀수 있음

---

# .env 설정

## dotenv

https://www.daleseo.com/js-node-process-env/

https://www.daleseo.com/js-dotenv/

- dotenv라는 라이브러리를 이용하면 .env파일을 Java의 application.yml처럼 쓸 수 있다
- dotenv 의존성을 추가하고 .env에 다양한 값들을 작성하면, process.env라는 객체에 저장된다
- process는 전역 객체로 어디서나 자유롭게 사용할 수있다
- dotenv는 nest.js에선 기본적으로 추가되어 있다
- 명령어로 넘긴 환경변수값이랑 .env 파일에 작성한 환경변수 값이 겹친다면, 명령어로 넘긴 값이 우선적용됨
    - 명령어로 작성한 `NODE_ENV=development PORT=8080 nest start` 와
      .env.dev 파일에 작성한 `NODE_ENV=dev`, `PORT=3000`가 겹치면,
      `process.env.NODE_ENV=development`, `process.env.PORT=8080`임

```bash
#파일 생성
.env
.env.local
.env.dev
.env.prod

#내용 작성
PORT=3000
DB_TYPE=
DB_HOST=
DB_PORT=
DB_USERNAME=
DB_PASSWORD=
DB_DATABASE=
DB_SCHEMA=
```

## package.json

```bash
"scripts": {
    "start": "nest start",
    "start:local": "NODE_ENV=local nest start",
    "start:dev": "NODE_ENV=dev nest start",
    "start:prod": "NODE_ENV=prod nest start"
  }
```

- 앱 실행 명령어를 간소화하는 내용이다
- NODE_ENV 환경변수값을 명령어에 포함시켜 넘겨주는데, 이 값을 토대로 사용할 .env 파일을 찾으므로, .env파일의 이름과 같은 값을 넘겨준다

## cross-env

```bash
npm install cross-env
```

```bash
"scripts": {
    "start": "cross-env nest start",
    "start:local": "cross-env NODE_ENV=local nest start",
    "start:dev": "cross-env NODE_ENV=dev nest start",
    "start:prod": "cross-env NODE_ENV=prod nest start"
  }
```

- dotenv가 윈도우에서는 안는데, 앞에 cross-env 붙이면 운영체제 가리지 않고 사용하게 해줌
- https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-cross-env-%EB%AA%A8%EB%93%88-%EC%82%AC%EC%9A%A9%EB%B2%95
- https://velog.io/@dinb1242/Nest.js-dotenv-%EC%99%80-cross-env-%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%ED%99%98%EA%B2%BD-%EB%B3%84-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EC%A7%80%EC%A0%95%ED%95%98%EA%B8%B0

## ConfigModule

- Nest.js는 NestConfig을 제공해, 각종 설정을 할 수 있도록 한다

```bash
# 의존성 추가
npm i --save @nestjs/config
```

- 의존성 추가 필요함

## app.module.ts

```tsx
@Module({
  imports: [
    MemberModule,
    ConfigModule.forRoot({
      // 실행할 .env 파일을 지정
      // 실행 명령어로 전달받은 NODE_ENV 값으로 파일명을 구분하여 해당 실행환경 로드
      envFilePath: process.env.NODE_ENV
        ? `.env.${process.env.NODE_ENV}`
        : '.env',

      // ConfigModule 전역 설정
      isGlobal: true,

      // Joi를 이용해 .env 값들을 검증
      validationSchema: Joi.object({
        NODE_ENV: Joi.string().valid('local', 'dev', 'prod').required(),
        PORT: Joi.number().port().required(),
        DB_TYPE: Joi.string().required(),
        DB_HOST: Joi.string().required(),
        DB_PORT: Joi.number().required(),
        DB_USERNAME: Joi.string().required(),
        DB_PASSWORD: Joi.string().required(),
        DB_DATABASE: Joi.string().required(),
      }),
    } as ConfigModuleOptions),

    // TypeORM 설정
    TypeOrmModule.forRoot({
      type: process.env.DB_TYPE,
      host: process.env.DB_HOST,
      port: Number(process.env.DB_PORT),
      username: process.env.DB_USERNAME,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_DATABASE,
      schema: process.env.DB_SCHEMA,
      synchronize: process.env.NODE_ENV === 'local',
    } as TypeOrmModuleOptions),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

- AppModule에서 @Module() 데코레이터로 도메인 레벨의 모듈을 넘어, 어플리케이션 레벨의 모듈들(ConfigModule, TypeOrmModule등)의 설정을 진행한다
- ConfigModule.forRoot({})로 어플리케이션 환경설정 진행
- TypeOrmModule.forRoot({})로 TypeORM 환경설정 진행

## Joi

- .env에서 저장된 파일의 유효성 검사, type safety check를 위해 사용하는 라이브러리 중 하나
- tsconfig.json에서 해당 옵션 추가해주어야함
    - `"esModuleInterop": true,`
    - es모듈과 commonjs쪽을 모두 상호 사용하게 해줌
    - https://velog.io/@jcs5679/esModuleInterop-true-%EC%98%B5%EC%85%98

```bash
# npm
npm install joi
```

```tsx
@Module({
  imports: [
    MemberModule,
    ConfigModule.forRoot({
      // 실행할 .env 파일을 지정
      // 실행 명령어로 전달받은 NODE_ENV 값으로 파일명을 구분하여 해당 실행환경 로드
      envFilePath: process.env.NODE_ENV
        ? `.env.${process.env.NODE_ENV}`
        : '.env',

      // ConfigModule 전역 설정
      isGlobal: true,

      // Joi를 이용해 .env 값들을 검증
      validationSchema: Joi.object({
        NODE_ENV: Joi.string().valid('local', 'dev', 'prod').required(),
        PORT: Joi.number().port().required(),
        DB_TYPE: Joi.string().required(),
        DB_HOST: Joi.string().required(),
        DB_PORT: Joi.number().required(),
        DB_USERNAME: Joi.string().required(),
        DB_PASSWORD: Joi.string().required(),
        DB_DATABASE: Joi.string().required(),
      }),
    } as ConfigModuleOptions),

    // TypeORM 설정
    TypeOrmModule.forRoot({
      type: process.env.DB_TYPE,
      host: process.env.DB_HOST,
      port: Number(process.env.DB_PORT),
      username: process.env.DB_USERNAME,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_DATABASE,
      schema: process.env.DB_SCHEMA,
      synchronize: process.env.NODE_ENV === 'local',
    } as TypeOrmModuleOptions),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

- Joi를 이용해 어플리케이션 실행시에 .env의 타입 및 필수값 검증을 진행 가능함
- ConfigModule.forRoot({ validationSchema: Joi.object({..}) })에 작성

## main.ts

```tsx
async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    //CORS
    cors: true,
  });

  await app.listen(process.env.PORT || 3000);
}

bootstrap();
```

- port, cors 등 어플리케이션 관련 전반적인 설정을 진행하는곳

---

# 도메인 모듈 리소스 생성

```bash
# npm
npm install -g @nestjs/cli
```

- Nest.js/cli 필요 (기본에 포함되어있는지는 기억이)

```bash
# 전체 리소스 생성
nest g res <domain>

# 컨트롤러
nest g co <domain>

# 서비스
nest g s <domain>

# 모듈
nest g mo <domain>

```

https://docs.nestjs.com/cli/usages

- 전체 리소스 생성시, API 형태에 맞게 선택 가능하고 dto 및 entities까지 생성해주므로 구조를 참고하기에도 좋음

---

# TypeORM

```bash
# typeorm, nestjs/typeorm 모듈
npm install --save typeorm @nestjs/typeorm

# postgres 모듈
npm install --save pg

# mariadb, mysql 모듈
npm install --save mysql2

# 한번에
npm install --save typeorm @nestjs/typeorm mysql2
npm install --save typeorm @nestjs/typeorm pg
```

- @nestjs/typeorm: Nest.js의 TypeORM 연결 모듈
- typeorm: TypeORM 모듈
- pg: PostgreSQL 모듈
- mysql2: MySQL, MariaDB 모듈

---

# DB 컨테이너 실행

```bash
# mariadb
docker pull mariadb:10.6

# mariadb run
docker run \
-d \
-p 3306:3306 \
-e "MYSQL_ROOT_PASSWORD=<password>" \
-e "TZ=Asia/Seoul" \
-v "</your/path/data>:/var/lib/mysql" \
-v "</your/path/config>:/etc/mysql" \
-v "</your/path/log>:/var/log/mysql" \
--network <your_network_name> \
--ip <static_ip_if_you_need> \
--restart="always" \
--name <your_container_name> \
mariadb:10.6

# postgres
docker pull postgres:15.10

# postgres run
docker run \
-d \
-p 5432:5432 \
-e "POSTGRES_USER=<name>" \
-e "POSTGRES_PASSWORD=<password>" \
-e "POSTGRESQL_REPLICATION_USE_PASSFILE=NO" \
-e "TZ=Asia/Seoul" \
-v "</your/path>:/var/lib/postgresql/data" \
--name <your_container_name> \
postgres:15.10
```