# package.json의 핵심 필드: files와 main

## 개요

`package.json`은 Node.js 프로젝트의 핵심 설정 파일로, 프로젝트의 메타데이터와 의존성을 관리합니다. 이 글에서는 패키지 배포와 모듈 진입점을 결정하는 중요한 두 필드인 `files`와 `main`에 대해 자세히 살펴보겠습니다.

## main 필드

### 정의와 목적

`main` 필드는 패키지의 진입점(entry point)을 지정하는 필드입니다. 다른 프로젝트에서 해당 패키지를 `require()` 또는 `import`할 때 실행되는 파일을 정의합니다.

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "main": "index.js"
}
```

### 동작 방식

1. **CommonJS 환경에서:**

   ```javascript
   const myPackage = require("my-package");
   // 이때 my-package/index.js가 실행됩니다
   ```

2. **ES Modules 환경에서:**
   ```javascript
   import myPackage from "my-package";
   // 이때 my-package/index.js가 실행됩니다
   ```

### 기본값과 규칙

- **기본값**: `index.js`
- **권장사항**:
  - 루트 디렉토리에 `index.js` 파일을 두는 것이 관례
  - 복잡한 패키지의 경우 `lib/index.js` 또는 `src/index.js` 사용

### 실제 예시

```json
{
  "name": "lodash",
  "version": "4.17.21",
  "main": "lodash.js"
}
```

```json
{
  "name": "express",
  "version": "4.18.2",
  "main": "index.js"
}
```

## files 필드

### 정의와 목적

`files` 필드는 npm 패키지에 포함될 파일들을 지정하는 배열입니다. 이 필드가 없으면 npm은 기본 포함 규칙을 따르며, 지정된 경우 해당 파일들만 패키지에 포함됩니다.

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "files": ["lib", "dist", "index.js", "README.md"]
}
```

### 기본 포함 규칙 (files 필드가 없을 때)

npm은 다음 파일들을 자동으로 포함합니다:

- `package.json`
- `README.md`
- `CHANGELOG.md`
- `LICENSE`
- `index.js` (main 필드에 지정된 파일)
- `bin` 디렉토리
- `man` 디렉토리

### 자동 제외 규칙

다음 파일들은 항상 제외됩니다:

- `.git`
- `.gitignore`
- `.npmrc`
- `.npmignore`
- `node_modules`
- `*.tgz`
- `*.tar.gz`
- `.DS_Store`
- `Thumbs.db`

### files 필드 사용 시 주의사항

1. **명시적 포함**: files 필드를 지정하면 기본 포함 규칙이 무시됩니다
2. **필수 파일**: `package.json`은 항상 포함되므로 별도 지정 불필요
3. **디렉토리 포함**: 디렉토리를 지정하면 해당 디렉토리의 모든 파일이 포함됩니다

### 실제 예시

```json
{
  "name": "react",
  "version": "18.2.0",
  "main": "index.js",
  "files": [
    "LICENSE",
    "README.md",
    "build-info.json",
    "index.js",
    "cjs/",
    "umd/"
  ]
}
```

```json
{
  "name": "typescript",
  "version": "5.0.4",
  "main": "lib/typescript.js",
  "files": ["lib", "bin", "README.md"]
}
```

## files와 main의 상호작용

### 시나리오 1: 기본 설정

```json
{
  "name": "simple-package",
  "version": "1.0.0",
  "main": "index.js"
}
```

- `index.js`가 진입점
- `files` 필드가 없으므로 기본 포함 규칙 적용

### 시나리오 2: 명시적 파일 지정

```json
{
  "name": "explicit-package",
  "version": "1.0.0",
  "main": "dist/index.js",
  "files": ["dist", "README.md"]
}
```

- `dist/index.js`가 진입점
- `dist` 디렉토리와 `README.md`만 패키지에 포함

### 시나리오 3: 복잡한 구조

```json
{
  "name": "complex-package",
  "version": "1.0.0",
  "main": "lib/index.js",
  "files": ["lib", "types", "README.md", "LICENSE"]
}
```

- `lib/index.js`가 진입점
- `lib`, `types` 디렉토리와 문서 파일들만 포함

## 모범 사례

### 1. main 필드

- **명확한 진입점**: 패키지의 주요 기능을 내보내는 파일을 지정
- **일관성**: 프로젝트 내에서 일관된 진입점 사용
- **문서화**: main 필드가 가리키는 파일의 역할을 명확히 문서화

### 2. files 필드

- **최소화**: 필요한 파일만 포함하여 패키지 크기 최적화
- **명시적**: 기본 포함 규칙에 의존하지 않고 명시적으로 지정
- **테스트**: `npm pack` 명령어로 실제 포함될 파일들 확인

### 3. 개발 워크플로우

```bash
# 패키지에 포함될 파일들 미리보기
npm pack --dry-run

# 실제 패키지 생성
npm pack
```

## 디버깅 팁

### 문제 해결

1. **모듈을 찾을 수 없는 경우:**

   - `main` 필드가 올바른 파일을 가리키는지 확인
   - 해당 파일이 실제로 존재하는지 확인

2. **예상한 파일이 패키지에 없는 경우:**

   - `files` 필드에 해당 파일/디렉토리가 포함되어 있는지 확인
   - `.npmignore` 파일이 해당 파일을 제외하고 있는지 확인

3. **패키지 크기가 너무 큰 경우:**
   - `files` 필드를 사용하여 불필요한 파일 제외
   - `npm pack`으로 실제 포함될 파일들 확인

## 결론

`main`과 `files` 필드는 npm 패키지의 핵심 구성 요소입니다:

- **main**: 패키지의 진입점을 정의하여 사용자가 패키지를 올바르게 사용할 수 있도록 함
- **files**: 패키지에 포함될 파일들을 제어하여 배포 크기를 최적화하고 보안을 강화

이 두 필드를 올바르게 설정하면 사용자 친화적이고 효율적인 npm 패키지를 만들 수 있습니다.
