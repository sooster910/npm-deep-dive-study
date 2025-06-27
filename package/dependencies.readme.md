# dependencies, devdependencies

td:lr

- dependencies와 devdependencies의 주요차이는 해당 패키지가 다른 패키지에서 설치될 때 설치되는가의 차이

## dependencies

npm을 사용하거나 개발하기위해 반드시 필요한 패키지

의존관계 나타내기
c 패키지를 루트에서 npm install로 실행하면 a패키지가 설치됨과 동시에 dependencies로 있던 b패키지도 설치된다.

불필요한 낭비를 막기위해서는 dependencies에 있는 패키지들이 실제로 사용되는지 잘 판단해서 적용

```json
{
    "name:"a",
    "dependencies":{
        "b":"1.1.0"
    }
}

```

```json
{
    "name:"c",
    "dependencies":{
        "a":"1.1.0"
    }
}

```

### 특정버전 선언시

- 버전이 정확하게 명시되어있기 때문에 npm update를 실행해도 버전이 변경되지 않는다. package-lock.json, node_modules에도 영향을 미치지 못한다.

## ^version캐럿

- 해당 버전과 호환되는 버전 까지만 의존한다.
- semantic-versioning에서 설명했듯이 호환되는 버전은 주버전이 변경되기 이전까지의 버전과 호환성을 가리킨다.
- 예를들어 `^16.8.0` 은 17이상은 지원하지 않음.

```json
{
  "dependencies": {
    "react": "^16.8.0"
  }
}
```

## ~version 틸트

해당 버전의 버그 수정, 즉 패치 버전의 변경까지만 용인함
~16.8.0 은 16.8.0부터 16.8.x까지 허용

```json
{
  "dependencies": {
    "react": "~16.8.0"
  }
}
```

## \*version

`*` 나 빈문자열이 있다면 아무버전이나 상관없음을 의미하며 npm install이나npm update를 실행했을 때 최신 버전을 설치한다.

항상 최신버전을 설치핮기 때문에, 기존의 프로젝트와 하위호환성에 유의해야 함.

## devdependencies

해당 패키지를 개발할 때만 필요한 패키지를 의미(개발자가 필요로 하는 패키지)
좀 더 풀어 이야기 하면 devDependencies는 해당 패키지를 "직접 개발"할 때만 설치됩니다.

아래처럼 a패키지를 설치하면 b패키지가 설치되지만 c패키지를 설치 할 때 b패키지는 설치 되지 않는다. b패키지가 devdependencies로 선언되어있기 때문이다.

```json
{
    "name:"a",
    "devdependencies":{
        "b":"1.1.0"
    }
}

```

```json
{
    "name:"c",
    "dependencies":{
        "a":"1.1.0"
    }
}

```

왜 이렇게 동작할까?

c 입장에서는 "a라는 완성된 패키지"만 필요하기 때문에 a를 개발할 때 필요했던 도구(b)는 c가 a를 사용할 때는 필요 없다.

## peer dependencies

책에서 작성한 부분을 조금 변형했다(개인적으로 이 스타일이 나한테는 이해하기 쉬웠기 때문)

특정패키지를 require하지 않는 경우에 사용할 수 있지만 꼭 그렇지만도 않다.

예시를 보고 이해해보기

```javascript
const useMultiply = (a, b) => a * b;

export default useMultiply;
```

react를 required하지 않지만, react를 사용하는 곳에서 사용될 목적으로 만들어 졌다면,react를 peerDependencies로 지정할 수 있다.

```json
{
    "peerDependencies":{
        "react":"^18.0.0
    }
}

```

react를 꼭 required하지 않을 때에만 peerDependenceis를 사용하는건 아니다.

react-use의 경우 react를 코드내에 완전히 의존

![react-use](./react-use.png)
