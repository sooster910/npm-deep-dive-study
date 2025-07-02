## npx

npm 패키지중에서 일부 패키지는 CLI도구처럼 직접 실행 할 수 있다.
npx는 특정 패키지를 로컬이나 전역에 설치하지 않고도 실행할 수 있게 하는 도구이다.
(npx는 평소에 사용하는 운영체제의 명령어(예: ls, cd, dir 등)처럼, Node.js 패키지 내부에 숨겨진 명령어들을 필요할 때 바로 꺼내 쓸 수 있도록 돕는 도구임)

npx를 무조건 모든 패키지에 사용할 수 있는 건 아니다.

### npx를 지원하는 상황

**1. bin 필드를 가지고 있는 패키지**
이게 가장 중요한 부분, 패키지 안에 터미널에서 직접 실행할 수 있는 명령어(bin 필드에 정의된)가 있을 때 npx가 제 역할을 한다. create-react-app, eslint, prettier처럼 우리가 CLI 도구로 사용하는 대부분의 패키지들이 여기에 해당한다.

**2. CLI (명령줄 인터페이스) 도구로 만들어진 패키지**

애초에 터미널에서 특정 작업을 수행하도록 설계된 패키지들을 실행할 때 npx가 아주 유용

### bin 필드

npx의 주요 기능 중 하나는 프로젝트의 node_modules/.bin 디렉토리에 있는 실행 파일을 쉽게 찾아 실행하는 것이다.

npm install을 통해 패키지가 설치될 때, bin 필드에 정의된 스크립트들은 심볼릭 링크(Symbolic Link)로 node_modules/.bin 디렉토리에 생성된다.
만약 패키지가 전역으로 설치되면, 해당 스크립트들은 시스템의 전역 실행 경로(예: /usr/local/bin 또는 C:\Users\{사용자명}\AppData\Roaming\npm)에 심볼릭 링크로 생성된다.

```json
// package.json
{
  "name": "my-cli-tool",
  "version": "1.0.0",
  "bin": {
    "my-command": "./bin/my-command.js"
  }
}
```

예를 들어, 위에서 정의한 my-command를 실행하고 싶다면, npx my-command라고 입력하면 된다. npx는 node_modules/.bin/my-command를 찾아 실행해준다.

이는 우리가 보통 node_modules/.bin/my-command라고 직접 입력하거나 package.json의 scripts에 정의해서 npm run my-command로 실행하는 것보다 훨씬 간편하다.
