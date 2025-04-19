### 1. Prettier 설치

프로젝트에서 Prettier를 사용하려면 먼저 설치가 필요하다:

```bash
npm install --save-dev prettier
```

---

### 2. Prettier 설정 파일 생성 (선택 사항)

Prettier 설정 파일을 프로젝트 루트에 추가해 설정을 관리할 수 있다. 아래 파일 중 하나를 생성하면 된다:

- `.prettierrc` 또는 `.prettierrc.json`
- `.prettierrc.js` (JavaScript 형식)
- `.prettierrc.yml` (YAML 형식)

**예: `.prettierrc`**

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all"
}
```

---

### 3. 프로젝트 내 파일 포맷팅

Prettier 명령어를 실행해 파일을 포맷팅한다:

**단일 파일 포맷팅:**

```bash
npx prettier --write path/to/your/file.js
```

**전체 프로젝트 포맷팅:**

```bash
npx prettier --write .
```

---

### 4. 특정 파일 형식만 포맷팅

Prettier는 파일 확장자별로 지정해 실행할 수 있다. 예를 들어 JavaScript와 TypeScript 파일만 포맷팅하려면:

```bash
npx prettier --write "**/*.{js,ts}"
```

---

### 5. Prettier를 ESLint와 통합 (선택 사항)

Prettier를 ESLint와 통합하려면 추가 패키지를 설치한다:

```bash
npm install --save-dev eslint-config-prettier
```

ESLint 설정 파일에 Prettier를 추가:

```json
{
  "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended", "prettier"]
}
```

---

### 6. Prettier 실행 자동화 (선택 사항)

Git 훅을 통해 코드 커밋 전에 자동으로 Prettier를 실행하려면 `husky`와 `lint-staged`를 사용할 수 있다:

```bash
npm install --save-dev husky lint-staged
```

**`package.json` 설정 예:**

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "**/*.{js,ts,json,css,md}": "prettier --write"
  }
}
```

---

### IntelliJ 코드 포맷팅시 자동 적용

- 서비스 → prettier 검색 → JavaScript 하위 prettier 확인 → Manual… 하위 Run on Reformat on action 체크

---

### 요약

- Prettier를 설치한 뒤 `npx prettier --write .` 명령어로 프로젝트를 포맷팅.
- `.prettierrc` 파일로 설정을 관리하고, 필요하면 ESLint 및 Git 훅과 통합해 자동화.