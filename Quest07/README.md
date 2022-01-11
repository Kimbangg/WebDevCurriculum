# Quest 07. node.js의 기초

## Introduction

- 이번 퀘스트에서는 node.js의 기본적인 구조와 개념에 대해 알아 보겠습니다.

## Topics

- node.js
- npm
- CommonJS와 ES Modules

## Resources

- [About node.js](https://nodejs.org/ko/about/)
- [Node.js의 아키텍쳐](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174356/node-js%EC%9D%98-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90)
- [npm](https://docs.npmjs.com/about-npm)
- [npm CLI commands](https://docs.npmjs.com/cli/v7/commands)
- [npm - package.json](https://docs.npmjs.com/cli/v7/configuring-npm/package-json)
- [How NodeJS Require works!](https://www.thirdrocktechkno.com/blog/how-nodejs-require-works)
- [MDN - JavaScript Modules](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules)
- [ES modules: A cartoon deep-dive](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)
- [require vs import](https://www.geeksforgeeks.org/difference-between-node-js-require-and-es6-import-and-export/)

## Checklist

- node.js는 무엇인가요? node.js의 내부는 어떻게 구성되어 있을까요?
- npm이 무엇인가요? `package.json` 파일은 어떤 필드들로 구성되어 있나요?
  - Node Pacackage Manager. 이하 npm은 이름 그대로 노드 패키지 매니저이다. 세상에는 많은 자바스크립트 프로그래머들이 있고, 그들이 유용한 자바스크립트 패키지들을 이미 만들어 두었고, 그런 코드들이 공개되어 있는 것이 바로 npm이다.
  - package.json 파일 구성
    - name, version
    - description
    - keywords: 키워드를 문자열 배열로 설명한다.
    - homepage: 프로젝트 홈페이지가 있을 경우에 입력한다.
    - peoplefields: author, contributors
    - main
    - bin
    - repository: 소스코드가 관리되는 저장소 위치를 지정한다.
    - devDependencies
    - dependency
- npx는 어떤 명령인가요? npm 패키지를 `-g` 옵션을 통해 글로벌로 저장하는 것과 그렇지 않은 것은 어떻게 다른가요?
  - `-g` 옵션을 사용하면 설치한 프로젝트 외에 다른 프로젝트에서도 해당 모듈을 사용할 수 있습니다.
  - 리눅스 환경에서는 sudo 명령어로 실행해야 하고, 윈도우는 관리자 권한이 필요합니다.
    <br><br>
- 자바스크립트 코드에서 다른 파일의 코드를 부르는 시도들은 지금까지 어떤 것이 있었을까요? CommonJS 대신 ES Modules가 등장한 이유는 무엇일까요?

  - 모듈 시스템

    - 최초의 자바스크립트는 아주 간단한 모듈 시스템만을 제공했습니다. 바로 HTML에서 자바스크립트 원본 소스를 제공하고, 브라우저에서 순서대로 로드하는 방식이었습니다.
    - 다만, 이 방법을 썼을 때는 전역 컨텍스트에서 각 모듈 간의ㅏ 충돌이 발생할 수 있습니다.
    - 즉, 모듈 간의 스코프가 구분이 되지 않아 다른 파일을 오염시키는 경우가 발생할 수 있습니다.

    ```
      <html>
        <script src="/src/foo.js"></script>
        <script src="/src/bar.js"></script>
        <script src="/src/baz.js"></script>
        <script src="/src/qux.js"></script>
        <script src="/src/quux.js"></script>
      </html>
    ```

  - Common JS

    - 이름에 나와있던 것처럼, 브라우저 뿐만이 아니라 서버 사이드나 데스크톱에서 범용적으로 사용하기 위한 모듈 시스템을 만들기 위한 자발적인 구조입니다.
    - 모듈 시스템이기 때문애, 모든 디펜던시가 로컬 디크스에 존재해서 필요한 모듈을 사용할 수 있는 환경을 전제로 합니다. 따라서, 동기적으로 모듈을 호출하는 방식을 선택하였습니다.
    - 다만, 비동기보다 속도가 느리고 트리 쉐이킹이 어렵다는 단점을 가지고 있었습니다.

    ```
      module.exports = foo;

      const foo = require("./foo");
    ```

  - AMD & Require

    - 비동기 상황에서도 자바스크립트 모듈을 사용하기 위해서 나온 그룹입니다.
    - 초창기에 AMD가 탄생한 이유도, 브라우저에서의 모듈 실행을 우선적으로 여겼기 때문입니다.

  - ES6 Module

    - 이전의 도전들이 궁극적인 목적인 `모듈 시스템의 부재` 를 해결하지 못하여 이를 해결하기 위해 등장한 것이 ES6 Module 입니다.
    - 문제를 보완하는 것에 포커스가 맞춰져 있었기에, [동기/비동기]를 모두 지원했고, 정적 분석이 가능했기 때문에 트리 쉐이킹도 편리하였습니다.
    - 문제점이 있다면, 최신화된 문법이다보니 IE 같은 구형 브라우저에서는 적용이 되지 않았습니다.

  - 모듈 번들링
    - 분리된 모듈을 합치고, 사용하지 않는 코드를 제거하는 최적화 작업을 하기위한 목적으로 탄생하였습니다.
    - 대표적인 예제로는 [웹팩, 파셀] 등이 있습니다.

## Quest

- 스켈레톤 코드에는 다음과 같은 네 개의 패키지가 있으며, 용도는 다음과 같습니다:
  - `cjs-package`: CommonJS 기반의 패키지입니다. 다른 코드가 이 패키지의 함수와 내용을 참조하게 됩니다.
  - `esm-package`: ES Modules 기반의 패키지입니다. 다른 코드가 이 패키지의 함수와 내용을 참조하게 됩니다.
  - `cjs-my-project`: `cjs-package`와 `esm-package`에 모두 의존하는, CommonJS 기반의 프로젝트입니다.
  - `esm-my-project`: `cjs-package`와 `esm-package`에 모두 의존하는, ES Modules 기반의 프로젝트입니다.
- 각각의 패키지의 `package.json`과 `index.js` 또는 `index.mjs` 파일을 수정하여, 각각의 `*-my-project`들이 `*-package`에 노출된 함수와 클래스를 활용할 수 있도록 만들어 보세요.

## Advanced

- node.js 외의 자바스크립트 런타임에는 어떤 것이 있을까요?
