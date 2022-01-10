# Quest 06. 인터넷의 이해

## Introduction

- 이번 퀘스트에서는 인터넷이 어떻게 동작하며, 서버와 클라이언트, 웹 브라우저 등의 역할은 무엇인지 알아보겠습니다.

## Topics

- 서버와 클라이언트, 그리고 웹 브라우저
- 인터넷을 구성하는 여러 가지 프로토콜
  - IP
  - TCP
  - HTTP
- DNS

## Resources

- [OSI 모형](https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95)
- [IP](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
  - [Online service Traceroute](http://ping.eu/traceroute/)
- [TCP](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
  - [Wireshark](https://www.wireshark.org/download.html)
- [HTTP](https://ko.wikipedia.org/wiki/HTTP)
  - Chrome developer **tool**, Network tab
- [DNS](https://ko.wikipedia.org/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%EB%84%A4%EC%9E%84_%EC%8B%9C%EC%8A%A4%ED%85%9C)
  - [Web-based Dig](http://networking.ringofsaturn.com/Tools/dig.php)

## Checklist

- 인터넷은 어떻게 동작하나요? Internet Protocol Suite의 레이어 모델에 입각하여 설명해 보세요.

  - 1:1 연결

    - 두 대의 컴퓨터가 통신을 원할 때 우리는 다른 컴퓨터와 [무선 | 유선] 으로 연결이 되어있어야 합니다.
    - 이 때 데이터는 전기 선을 타고 이동하기 때문에, 데이터를 [0,1] 의 전기신호로 변경 해줘야한다.
    - 받은 전기신호를 데이터로 변경(=Decode)하거나 데이터를 전기신호로 바꾸는(=Encode)는 모두 물리계층에서 수행한다.

  - N:N (네트워크 내부)

    - 1:1 연결 방식처럼, 직접 연결을 하기 위해서는 10대 기준 45개의 전선이 필요한데, 전세계에 존재하는 컴퓨터를 다 세워본다면 과연 몇 대이며, 몇 개의 전선이 필요할까?
    - 결과적으로 전선도 비용이기 때문에 이를 최소화 하기 위한 방법들을 고민했고 끝내 스위치가 탄생 하였습니다.
      - 여기서 스위치란, 전선을 뭉쳐둘 수 있는 박스라고 생각하시면 편할 것 같습니다.
    - 스위치의 도입과 함께, 생긴 첫 번째 문제는 A에게 보내고자 하는 데이터가 같은 AS내에 있는 컴퓨터에게 모두 전달된다는 것이였는데, 이는 라우터가 목적지를 확인하고 데이터를 넘겨주는 방식으로 해결이 가능했다.
    - 두 번째 문제점은 다양한 곳에서 데이터가 온다는 것이였는데 이 또한 프레임을 통해 (데이터 앞뒤로 고유한 값을 추가하는) 해결하였다.

  - N:N (네트워크 외부)
    - 내가 속한 네트워크가 아닌 곳에 데이터를 보내기 위해서는 라우터가 필요합니다.
    - 보내고자 하는 상대의 IP 주소를 바탕으로, 길을 찾고(Routing) 자신의 다음 라우터에게 데이터를 넘겨주는 방식(Forwarding)을 통해 통신할 수 있습니다.

  * 두 전자기기가 신뢰성을 가지고 통신할 수 있도록 하기 위한 프로토콜은 어떻게 동작할까요?

  * HTTP
    - HTTP란?
      - HTTP란, Hyper Text Markup Language의 약자로써 웹 페이지를 만들기 위해서 웹 브라우저 위에서 동작하는 HTML로 만든 웹페이지를 어떻게 주고 받을 것인가? 에 대한 규약이다.

  - HTTP의 특징

    - 요청/응답
      - 클라이언트에서 요청을 보내면 서버는 요청을 처리해 응답한다.
    - Stateless

      - 클라이언트의 요청 기록이 남아있지 않는다.
      - 이를 통해서 불특정 다수를 대상으로 하는 서비스에 적합하고 접속 유지를 최소화함으로써 더 많은 유저의 요청 처리를 할 수 있다.
      - 하지만, 위에서 언급했듯 클라이언트의 상태를 알 수 없기 때문에 쿠키 등을 통해 이를 극복할 수 있다.

    - HTTP 1.0 vs 1.1 의 차이
      - 커넥션 유지
        - 1.0의 경우 데이터를 전송한 뒤에는 세션을 곧바로 끊지만, 1.1의 경우는 TCP 세션을 지속적으로 유지한다.(Persistent)
      - 파이프라이닝
        - HTTP 요청을 하는 경우, 파이프라이닝이 없을 때는 순차적으로 데이터를 주고받는다.
        - 즉, 요청#1에 대한 응답#1을 받아야 요청#2로 넘어갈 수 있는데 이 때 만약 이전 요청이 올바르게 처리되지 않는다면 어떻게 될까?
        - 이를 극복하기 위한 개념이 파이프라이닝으로 다수의 요청을 한 번에 전송하여 이에 대한 응답을 각기 수행한다.
      - 호스트 헤더 ( Virtual Hosting)
        - 도메인 마다 IP를 준비해야 하는데, Host 헤더의 추가를 통해서 Virtual 호스팅이 가능해졌다.

- 우리가 브라우저의 주소 창에 www.knowre.com 을 쳤을 때, 어떤 과정을 통해 서버의 IP 주소를 알게 될까요?
  - https://kimbangg.tistory.com/259

## Quest

- tracert(Windows가 아닌 경우 traceroute) 명령을 통해 www.google.com 까지 가는 경로를 찾아 보세요.
  - 어떤 IP주소들이 있나요?
  - 그 IP주소들은 어디에 위치해 있나요?
- Wireshark를 통해 www.google.com 으로 요청을 날렸을 떄 어떤 TCP 패킷이 오가는지 확인해 보세요
  - TCP 패킷을 주고받는 과정은 어떻게 되나요?
  - 각각의 패킷에 어떤 정보들이 담겨 있나요?
- telnet 명령을 통해 http://www.google.com/ URL에 HTTP 요청을 날려 보세요.
  - 어떤 헤더들이 있나요?
  - 그 헤더들은 어떤 역할을 하나요?

## Advanced

- HTTP의 최신 버전인 HTTP/3는 어떤 식으로 구성되어 있을까요?
- TCP/IP 외에 전세계적인 네트워크를 구성하기 위한 다른 방식도 제안된 바 있을까요?
