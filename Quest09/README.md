# Quest 09. 서버와 클라이언트의 대화

## Introduction

- 이번 퀘스트에서는 서버와 클라이언트의 연동, 그리고 웹 API의 설계 방법론 중 하나인 REST에 대해 알아보겠습니다.

## Topics

- expressJS, fastify
- AJAX, `XMLHttpRequest`, `fetch()`
- REST, CRUD
- CORS

## Resources

- [Express Framework](http://expressjs.com/)
- [Fastify Framework](https://www.fastify.io/)
- [MDN - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [REST API Tutorial](https://restfulapi.net/)
- [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
- [MDN - CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## Checklist

- 비동기 프로그래밍이란 무엇인가요?

  - 자바스크립트는 싱글 스레드 언어로, 한 번에 하나의 일만 처리할 수 있습니다. 즉 동기적으로만 수행할 수 있습니다.
  - 하지만, 우리가 만나는 웹은 동시에 여러 가지 일을 수행하는 것처럼 보이는데요. 이는 WebAPI, Event Loop 등 자바스크립트 런타임 환경의 도움을 받아서 수행이 가능한 것 입니다.
  - 만약 자바스크립트 엔진에서 처리할 수 없는 작업이 발생하면 -> Web API(DOM, 네트워크 요청, 브라우저 고유 기능 수행)에게 작업을 넘기고 -> Web API는 콜백 큐로 작업을 넘겨줍니다 -> 이벤트 루프는 콜백 큐의 작업이 완료되면 이를 콜스택으로 다시 올려줍니다.
    <br><br>

- 콜백을 통해 비동기적 작업을 할 때의 불편한 점은 무엇인가요? 콜백지옥이란 무엇인가요?

  - 만약 다음과 같은 코드를 작성한다면, 어떤 결과가 나올까요?

  ```
    var userData;

    function setUserData(data) {
      userData = someProcess(data);
    };

    $.get('http://www.example.org', function(response) {
      setUserData(response);
    })

    console.log(userData);
  ```

  - 예상하시는 내용처럼, 요청을 처리하기 전에 console.log가 먼저 처리가 되어 `undefined` 가 출력이 될겁니다.

  - 이를 해결하기 위해 다음과 같이 콜백을 쓰게된다면, 데이터를 가져온 다음에 userData에 있는 값이 출력이 될 것이기 때문에 정상적으로 나오는 모습을 볼 수 있습니다.

  ```
    var userData;

    function setUserData(data, callback) {
      userData = someProcess(data);
      callback(userData);
    };

    $.get('http://www.example.org', function(response) {
      setUserData(response, function(result) {
        console.log(result);
      });
    })
  ```

  - 하지만, 위와 같이 콜백이 2~3개 정도된다면 가독성이 나쁘지 않지만 콜백이 많아진다면 코드가 매우 복잡해지는 현상이 발생합니다.

- 자바스크립트의 Promise는 어떤 객체이고 어떤 일을 하나요?

  - 그리하여 위와 같은 콜백 지옥을 해결하기 위해 생겨난 것이 `Promise` 객체입니다.
  - 프로미스는 비동기 연산을 감싸는 일종의 프록시 객체로 [대기, 이행, 거부] 총 3가지 상태를 가집니다.

    - 대기 ( Pending )

      - 먼저 아래와 같이 new Promise() 메서드를 호출하면 대기 상태가 됩니다.

        new Promise();

    - 이행 (Fulfilled)

      - 콜백 함수의 인자 `resolve`를 아래와 같이 실행하면 이행 상태가 됩니다.
      - 이행 상태가 되면 then()을 이용하여 처리 결과 값을 받을 수 있습니다.

      ```
        function getData() {
        return new Promise((resolve, reject) => {
          $.get('http://www.example.org', function(response) {
            if (response) {
              resolve(response);
            }
            reject(new Error('error'));
          });
        });
      }

      getData()
        .then(task)
        .then(someTask)
        .then(someNextTask)
        .then(someNextNextTask);
      ```

    - 실패 (Rejected)

      - 실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 catch()로 받을 수 있습니다.

      ```
        function getData() {
          return new Promise(function(resolve, reject) {
          reject(new Error("Request is failed"));
          });
        }

        // reject()의 결과 값 Error를 err에 받음
        getData().then().catch(function(err) {
          console.log(err); // Error: Request is failed
        });
      ```

- 자바스크립트의 `async`와 `await` 키워드는 어떤 역할을 하며 그 정체는 무엇일까요?

  - 위에서 다뤘던 [콜백, Promise] 모두 우리가 사고하는 방식과 다르게 `비동기식` 사고를 요구하는 모습을 볼 수 있습니다.
    ```
      function logName() {
        // 아래의 user 변수는 위의 코드와 비교하기 위해 일부러 남겨놓았습니다.
        var user = fetchUser('domain.com/users/1', function(user) {
          if (user.id === 1) {
            console.log(user.name);
          }
        });
      }
    ```
  - 비동기식 사고에 익숙해졌다면 이해하는데는 큰 문제가 없겠지만, then 체이닝이 길어진다면 또는 콜백 지옥에 빠지게 된다면 우리는 그 코드를 이해하는데 정말 많은 시간을 투자해야 할 것입니다.
  - 이러한 불편함을 해결하기 위해서, 코드의 스타일을 개선한 방식이 async-await 입니다.
  - 위의 코드를 async-await로 바꿔본다면, 다음과 같이 동기적인 코드의 형태로 비동기 처리를 할 수 있게 됩니다.
    ```
      // 비동기 처리를 콜백으로 안해도 된다면..
      function logName() {
        var user = fetchUser('domain.com/users/1');
        if (user.id === 1) {
          console.log(user.name);
        }
      }
    ```

- 브라우저 내 스크립트에서 외부 리소스를 가져오려면 어떻게 해야 할까요?

  - 브라우저의 `XMLHttpRequest` 객체는 무엇이고 어떻게 동작하나요?

    - HTTP를 통해서 쉽게 데이터를 받을 수 있게 해주는 오브젝트를 제공한다.

    ```
    // 1. XMLHttpRequest()를 통해 오브젝트 생성을 한다.
    // 2. 그 후 open() 함수를 통해 메소드 방식, url, 그리고 비동기 여부를 설정한다.
    // 3. send(data) 로 설정한 방식대로 request를 보낸다.(data는 null이어도 된다.)

      var xhr = new XMLHTTPRequest();
      xhr.open('GET', 'http://localhost:3000/', true);
      xhr.send(null);

      xhr.onreadystatechange = function () {
        if ( xhr.readyStatte == XMLHTTPRequest.Done && xhr.status === 200 ) {
          console.log(xhr.responseText);
        }
      }
    ```

  - `fetch` API는 무엇이고 어떻게 동작하나요?

    - XMLHTTPRequest와 fetch API 모두 AJAX를 이용하여 전체 페이지를 새로고침 하지 않더라도 페이지의 일부의 데이터를 로드하기 위해서 사용이 되어왔다.
    - 하지만 fetch는 promise를 기반으로 구성이 되어있어서 XMLHTTPRequest보다 더 간편하게 사용할 수 있다는 장점이 존재한다.

    ```
      export const request = async (url, options = {}) => {
          try {
            const res = await fetch(`${API_END_POINT}${url}`, {
              ...options,
              headers: {
                'Content-Type': 'application/json',
              },
            })

            if (res.ok) {
              // resolved Promise
              return await res.json()
            }
            // rejected된 에러는 여기서 throw
            throw new Error('API 처리 중 문제가 발생했습니다')
          } catch (e) {
            alert(e.message)
          }
        }
    ```

- REST는 무엇인가요?
  - Representational State Transfer의 약자로, 자원을 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.
  - 즉, 자원의 표현에 의한 상태 전달(DB 학생 정보가 자원일 때, students를 자원의 표현으로 정한다.)
  - HTTP URI(Uniform Resource Indentifier)를 통해 자원을 명시하고, HTTP Method를 통해 해당 자원에 대한 CRUD를 적용하는 것을 의미한다.
    <br><br>
- RestFul API의 6가지 특징
  - 1- Uniform Interface
  - 2- Client-Server
    - 클라이언트 서버 구조를 가짐으로써, 관심사의 분리를 강요한다.
  - 3- Stateless
    - 클라이언트로부터 각각의 요청은 반드시 요청을 이해하고, 완료할 수 있는 필수적인 요소들이 포함이 되어있어야한다.
    - 서버는 과거의 정보를 통해 어떠한 이득도 얻을 수 없어야한다.
  - 4- Cacheable
    - 응답은 반드시 Cache가 가능한지 여부에 대한 라벨링을 해주어야한다.
  - 5- Layered System
  - 6- Code on Demand
    <br><br>
- REST API는 어떤 목적을 달성하기 위해 나왔고 어떤 장점을 가지고 있나요?
  - HTTP 프로토콜의 인프라를그대로 사용하므로 REST API 사용을 위한 별도 구축잎 필요없다.
  - 메세지가 의도하는바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
  - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- RESTful한 API 설계의 단점은 무엇인가요?
  - 사용할 수 있는 메서드가 4가지 밖에 없다.
  - 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 있다.
- CORS란 무엇인가요? 이러한 기능이 왜 필요할까요? CORS는 어떻게 구현될까요?
  - 교차출처리소스공유는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 어플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제를 의미합니다.
  - 브라우저와 서버 간의 안전한 교차 출처 요쳥 및 데이터 전송을 지원합니다.
- CORS는 왜 탄생하게 되었을까요?
  - 브라우저의 개발자 도구를 열면 DOM이 어떻게 작성되었는지, 리소스의 출처는 어딘지 등 중요한 정보들을 쉽게 확인 할 수 있습니다.
  - 이 때, 서로 다른 출처를 가진 어플이 아무 제약도 가지지 않고 통신이 가능하다면 정보 탈취하기가 쉬워지고 일반 사용자를 악성 사이트로 유도하는 등의 악의적인 행동이 일어날 수 있습니다.
- CORS는 어떻게 동작할까요?

  - 웹은 다른 출처의 리소스를 요청할 때는 HTTP 프로토콜을 사용하여 요청하는데, 이 때 브라우저는 요청 헤더에 Origin 필드에 요청을 보내는 출처를 담아 전송합니다.
  - 서버는 요청에 대한 응답을 하는데, 응답 헤더에 Access-Control-Allow-Origin 이라는 값에 `이 리소스에 접근하는 것이 허용된 출처` 를 내려줍니다.
  - 이 후, 응답을 받은 브라우저는 자신이 보냈던 요청의 Origin과 Access-Control-Allow-Origin 을 비교해 본 후 응답의 유효성을 확인합니다.

- Simple Request

  - CORS Prefilght를 거치지 않는 요청이다.
  - HTTP Method는 [GET, HEAD, Post] 중에 하나여야 한다.

- Prefilght Request
  - 브라우저는 요청을 예비 | 본 요청으로 나누어 서버에 전송하게 된다.
  - Prefilght 에서는 OPTIONS 메서드를 통해 다른 도메인의 리소스로 HTTP 요청을 보내 실제 요청이 안전한지 확인한다.

## Quest

- 이번 퀘스트는 Midterm에 해당하는 과제입니다. 분량이 제법 많으니 한 번 기능별로 세부 일정을 정해 보고, 과제 완수 후에 그 일정이 얼마나 지켜졌는지 스스로 한 번 돌아보세요.
  - 이번 퀘스트부터는 skeleton을 제공하지 않습니다!
- Quest 05에서 만든 메모장 시스템을 서버와 연동하는 어플리케이션으로 만들어 보겠습니다.
  - 클라이언트는 `fetch` API를 통해 서버와 통신합니다.
  - 서버는 8000번 포트에 REST API를 엔드포인트로 제공하여, 클라이언트의 요청에 응답합니다.
  - 클라이언트로부터 온 새 파일 저장, 삭제, 다른 이름으로 저장 등의 요청을 받아 서버의 로컬 파일시스템을 통해 저장되어야 합니다.
    - 서버에 어떤 식으로 저장하는 것이 좋을까요?
  - API 서버 외에, 클라이언트를 띄우기 위한 서버가 3000번 포트로 따로 떠서 API 서버와 서로 통신할 수 있어야 합니다.
  - Express나 Fastify 등의 프레임워크를 사용해도 무방합니다.
- 클라이언트 프로젝트와 서버 프로젝트 모두 `npm i`만으로 디펜던시를 설치하고 바로 실행될 수 있게 제출되어야 합니다.
- 이번 퀘스트부터는 앞의 퀘스트의 결과물에 의존적인 경우가 많습니다. 제출 폴더를 직접 만들어 제출해 보세요!
