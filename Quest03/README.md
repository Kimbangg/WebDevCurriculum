# Quest 03. 자바스크립트와 DOM

## Introduction

- 자바스크립트는 현재 웹 생태계의 근간인 프로그래밍 언어입니다. 이번 퀘스트에서는 자바스크립트의 기본적인 문법과, 자바스크립트를 통해 브라우저의 실제 DOM 노드를 조작하는 법에 대하여 알아볼 예정입니다.

## Topics

- 자바스크립트의 역사
- 기본 자바스크립트 문법
- DOM API
  - `document` 객체
  - `document.getElementById()`, `document.querySelector()`, `document.querySelectorAll()` 함수들
  - 기타 DOM 조작을 위한 함수와 속성들
- 변수의 스코프
  - `var`, `let`, `const`

## Resources

- [자바스크립트 첫걸음](https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps)
- [자바스크립트 구성요소](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks)
- [Just JavaScript](https://justjavascript.com/)

## Checklist

- 자바스크립트는 버전별로 어떻게 변화하고 발전해 왔을까요?

  - 표준 제정화의 필요
    - 넷스케이프에서는 자바스크립트, 그에 대응하기 위해서 MS에서는 J스크립트를 만들었습니다. 이처럼 브라우저마다 지원하는 언어가 달랐기 때문에 호환이 되지 않는 현상이 발생했습니다.
  - ECMA의 탄생
    - 위에서 발생한 파편화 현상을 막기 위해서 넷스케이프에서는 ECMA라는 국제 표준화 기구를 설립합니다.
    - ECMA에서 제작한 ECMASCript(ES)를 설립하였고, 브라우저에서 사용되는 자바스크립트, 제이스크립트 등은 ES를 따르게 되었습니다.
    - 자동차로 비유하면 쉽게 할 수 있는데, 언어는 차종이고 ES는 엔진이라고 생각하시면 좋을 것 같습니다. ( 외형은 달라도, 내부 구조는 같다는 의미 )
  - ES 1 ~ 3
    - 자바스크립트의 기본이 되는 기능만 존재하는 버전이다.
    - 호이스팅, 프로토타입, 스코프 등이 예시이다.
  - ES5
    - JSON parse / serialization과 Array, Object등에 많은 prototype method가 추가되었고 strict 모드가 추가되었다.
    - 가장 중요한 내용은 strict 모드인데, 기존에 너무 자유롭던 코드에서 일부 코드를 사용하지 못하게 막음과 동시에 더 많은 예외를 제공하여 프로그래머들이 더욱 안전한 코드를 작성할 수 있는 환경이 마련되었다.
  - ES6
    - 현재 자바스크립트의 표준으로 사용되고있는 버전이다.
    - ES6가 표준이 된 이유는 이전에 발생되었던 많은 문제들이 없어졌다는 점입니다.
    - 구조분해할당, 객체 초기자(키, 밸류 같으면 생략), 템플릿 리터럴, Default Parameter, Async/Await, Promise 등 아름다운 코드를 작성할 수 있는 다양한 기능들이 추가되었습니다.
    - 언어 외적으로도 [리액트, 뷰] 등의 유명 라이브러리가 ES6에 맞춰진 개발환경을 구축하였습니다.

<br>

- 자바스크립트의 버전들을 가리키는 ES5, ES6, ES2016, ES2017 등은 무엇을 이야기할까요?
  - ECMA는 매년 6월 새로운 ECMAScript에 대한 명세를 발표합니다.
  - 5, 6, 2016 등은 몇 번째로 발행한 명세 또는 명세 시기가 언제인지를 나타냅니다.
- 자바스크립트의 표준은 어떻게 제정될까요?
  - ECMASCript 표준을 담당하는 TC(=Technical Committee)39 참여자들에 의해 결정됩니다.
  - TC39 멤버들은 오래 일을 한 개발자 또는 교수 등 개발에 전문가인 사람들이 주를 이루어 토론하고, 만장일치 방식으로 새로운 규칙을 제정합니다.

<br>

- 자바스크립트의 문법은 다른 언어들과 비교해 어떤 특징이 있을까요?
  - 타입의 선언없이 사용이 된다.<br>
    (=> 간편함이 장점일 수는 있겠지만, 타입때문에 발생하는 문제들이 많다.)
  - 문자열의 하나의 타입으로 간주된다. <br>
    (=> C의 경우 단독 문자은 Char, 문자열은 문자를 배열에 담아둔 형태로 사용한다.)
  - 자바스크립트에서 반복문을 돌리는 방법은 어떤 것들이 있을까요? - forEach, map, for loop, while
    <br><br>
- 자바스크립트를 통해 DOM 객체에 CSS Class를 주거나 없애려면 어떻게 해야 하나요?
  - querySelector를 통해 Dom 객체에 접근을 한뒤 dom.style.background =""로 CSS 효과를 준다.
  - dom.classList.add("style")을 통해 CSS Class 추가.
  - dom.classList.remove() 를 통해 CSS Class 제거.
  - IE9나 그 이전의 옛날 브라우저들에서는 어떻게 해야 하나요?
    - babel을 통해서 ES3 ~ ES5 형식으로 Polyfill 해야합니다.
      <br><br>
- 자바스크립트의 변수가 유효한 범위는 어떻게 결정되나요?

  - 자바스크립트 변수는 [지역, 전역] 변수로 나뉘게 됩니다.
  - 지역 변수의 경우 함수 내부에 선언이 되는 변수로써, 범위는 함수 내부로 정해집니다.
  - 전역 변수의 경우, 전역에 작성된 변수를 뜻하며 지역/전역에서 모두 호출이 가능합니다.
    <br>

* `var`과 `let`으로 변수를 정의하는 방법들은 어떻게 다르게 동작하나요?

  - var는 함수 스코프, let은 블록(={}) 스코프를 가지고 있습니다.

  ```
   function run() {
    var foo = "Foo";
    let bar = "Bar";

    console.log(foo, bar); // Foo Bar

    {
      var moo = "Mooo"
      let baz = "Bazz";
      console.log(moo, baz); // Mooo Bazz
    }

    console.log(moo); // Mooo
    console.log(baz); // ReferenceError
  }

  run();
  ```

  ```
    for ( var i = 0; i < 3; i++) {
      console.log(i) // 2, 2, 2
    }
  ```

  ```
    for ( let i = 0; i < 2; i++ ) {
      console.log(i); // 0, 1, 2
    }
  ```

- 자바스크립트의 익명 함수는 무엇인가요?

  - 자바스크립트의 익명함수는 함수명 대신 변수명에 함수 코드를 저장하는 구현 방식입니다.
  - 익명 함수를 쓰더라도 작동이 가능한 이유는 함수명은 식별자로써 역할을 수행할 수 없기 때문에, 함수만 선언을 하더라도 함수명과 같은 형태의 함수 표현식을 만듭니다.
  - 고로 익명 함수는 결과적으로 변수명이라는 식별자로 호출이 되기 때문에, 이름이 없어도 무관합니다.

  ```
    var 변수명 = function (para) {
      ...
    }
  ```

  - 자바스크립트의 Arrow function은 무엇일까요?

    - 아래와 같은 형식의 익명함수입니다.
    - function, return을 모두 생략할 수 있습니다.
    - 화살표 함수의 this는 호출되는 시점이 아닌 선언되는 시점에 결정되며 언제나 상위 스코프의 this를 가르킵니다.

    ```
      var obj = {
        myName: 'Barbie',
        logName: () => {
          console.log(this.myName);
        }
      };

      obj.logName();   //undefined
    ```

    ```
      var status = "😎";

      setTimeout(() => {
        const status = "😍";

        const data = {
          status: "🥑",
          getStatus: function() {
            return this.status;
          }
        };

      console.log(data.getStatus.call(this));        //😎
    ```
