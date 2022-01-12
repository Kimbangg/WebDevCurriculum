# Quest 08. 웹 API의 기초

## Introduction

- 이번 퀘스트에서는 웹 API 서버의 기초를 알아보겠습니다.

## Topics

- HTTP Method
- node.js `http` module
  - `req`와 `res` 객체

## Resources

- [MDN - Content-Type Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
- [MDN - HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [MDN - MIME Type](https://developer.mozilla.org/en-US/docs/Glossary/MIME_type)
- [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
- [HTTP Node.js Manual & Documentation](https://nodejs.org/api/http.html)

## Checklist

- HTTP의 GET과 POST 메소드는 어떻게 다른가요?
  - GET 요청
    - 데이터를 읽거나 검색할 때, 사용되는 메소드입니다.
    - 요청 시, 필요한 데이터를 Body에 담지 않고, 쿼리스트링(URL?name=kim&age=23)을 통해 전송합니다.
    - 길이 제한을 가지고 있습니다.
    - 동일한 요청이 발생할 때, 서버로 요청을 보내지 않고 캐시된 데이터를 사용합니다.
    - IdemPotent한 성질을 가진 Get은 요청을 여러 번 수행해도 동일한 결과가 나와야합니다.
      - 문제점
        - 주소창에 사용자가 입력한 정보가 그대로 노출된다.
        - 이미지, 동영상과 같은 바이너리 파일의 데이터는 URL에 붙여서 보낼 수 없다.
          - => 이와 같은 문제는 POST 메소드를 사용하여 해결이 가능하다.
  - POST 요청
    - 주로 새로운 리소스를 생성 또는 변경하기 위해 사용되는 메소드입니다.
    - 전송 해야 될 데이터를 HTTP Body에 담아서 전송합니다.
    - 요청을 보낼 때는 Content-Type에 요청 데이터의 타입을 표시해야 합니다. 표시하지 않으면 서버는 내용이나 URL에 포함된 리소스의 확장자명 등으로 데이터 타입을 유추합니다.
    - Post는 non-Idempotent 한 성질 덕분에, 요청에 따른 응답이 항상 다를 수 있습니다.
  * 다른 HTTP 메소드에는 무엇이 있나요?
    - PUT 요청
      - 서버에 데이터를 전송하여, 자원을 생성하거나 업데이트 할 목적으로 사용됩니다.
      - POST요청과의 차이점은 idempotent한 성질로, 여러 번 PUT요청을 하더라도 응답 값이 같아야합니다.
- HTTP 서버에 GET과 POST를 통해 데이터를 보내려면 어떻게 해야 하나요?
  - HTTP 요청의 `Content-Type` 헤더는 무엇인가요?
    - HTTP Request Payload의 타입을 명시하기 위해서 사용이 됩니다.
  - Postman에서 POST 요청을 보내는 여러 가지 방법(`form-data`, `x-www-form-urlencoded`, `raw`, `binary`) 각각은 어떤 용도를 가지고 있나요?
    - form-data(양식 데이터)
      - Key-value 쌍으로 정해진 양식에 맞춰 데이터를 보낼 수 있도록 합니다.
    - x-www-form-urlencoded
      - 위의 방식으로 전송될 때는 인코딩이 되어서 데이터를 보낼 수 있도록 합니다.
- node.js의 `http` 모듈을 통해 HTTP 요청을 처리할 때,`req`와 `res` 객체에는 어떤 정보가 담겨있을까요?
  - 클라이언트의 요청 정보(url, method..)가 req에 담기고, 서버의 응답 값이 res에 담깁니다.

## Quest

- 다음의 동작을 하는 서버를 만들어 보세요.
  - 브라우저의 주소창에 `http://localhost:8080`을 치면 `Hello World!`를 응답하여 브라우저에 출력합니다.
  - 서버의 `/foo` URL에 `bar` 변수로 임의의 문자열을 GET 메소드로 보내면, `Hello, [문자열]`을 출력합니다.
  - 서버의 `/foo` URL에 `bar` 키에 임의의 문자열 값을 갖는 JSON 객체를 POST 메소드로 보내면, `Hello, [문자열]`을 출력합니다.
  - 서버의 `/pic/upload` URL에 그림 파일을 POST 하면 서버에 보안상 적절한 방법으로 파일이 업로드 됩니다.
  - 서버의 `/pic/show` URL을 GET 하면 브라우저에 위에 업로드한 그림이 뜹니다.
  - 서버의 `/pic/download` URL을 GET 하면 브라우저에 위에 업로드한 그림이 `pic.jpg`라는 이름으로 다운로드 됩니다.
- expressJS와 같은 외부 프레임워크를 사용하지 않고, node.js의 기본 모듈만을 사용해서 만들어 보세요.
- 처리하는 요청의 종류에 따라 공통적으로 나타나는 코드를 정리해 보세요.

## Advanced

- 서버가 파일 업로드를 지원할 때 보안상 주의할 점에는 무엇이 있을까요?
