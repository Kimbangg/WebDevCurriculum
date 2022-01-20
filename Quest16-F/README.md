# Quest 16-F. 컴포넌트 기반 개발

## Introduction

- 이번 퀘스트에서는 Vue.js 프레임워크를 통해 컴포넌트 기반의 웹 클라이언트 개발 방법론을 더 자세히 알아보겠습니다.

## Topics

- Vue.js framework
- vuex
- Virtual DOM

## Resources

- [Vue.js](https://vuejs.org)
  - [Lifecycle Hooks](https://v3.vuejs.org/guide/composition-api-lifecycle-hooks.html)
  - [State Management](https://v3.vuejs.org/guide/state-management.html)
  - [Virtual DOM](https://v3.vuejs.org/guide/optimizations.html#virtual-dom)

## Checklist

- Vue.js는 어떤 특징을 가지고 있는 웹 프레임워크인가요?

  - 템플릿을 제공하는 웹 프레임워크로, MVVM 패턴을 기반으로 만들어졌습니다.
  - Template이 HTML 과 유사하여 접근하기가 용이합니다.

- Vue.js는 내부적으로 어떤 식으로 동작하나요?

  - Virtual DOM을 사용하여 성능 최적화에 기여한다.
    - DOM을 최소한으로 조작하여 작업을 처리하는 방식
    - 가상 DOM이라는 DOM 트리를 모방한 가벼운 JS 객체를 통해 직접 DOM을 핸들링 하지 않고 자바스크립트가 HTML을 렌더링하는 방법

- Vue.js에서의 컴포넌트란 무엇인가요?

  - 조합하여 화면을 구성할 수 있는 브록을 의미합니다.
  - 컴포넌트를 활용하면 화면을 빠르게 구조화하여 일괄적인 패턴으로 개발할 수 있으며, 코드를 쉽게 이해하고 재사용할 수 있습니다.

- 컴포넌트 간에 데이터를 주고받을 때 단방향 바인딩과 양방향 바인딩 방식이 어떻게 다르고, 어떤 장단점을 가지고 있나요?
  - 양방향 데이터 바인딩
    - 화면에 표시되는 값과 프레임워크 모델 데이터 값이 동기화 -> 한 쪽이 변경되면 다르 한 쪽도 자동으로 변경
  - 단방향 데이터 흐름
    - 컴포넌트 단방향 통신 구조화 ( 상위 -> 하위 컴포넌트 )

## Quest

- Vue.js를 통해 메모장 시스템을 다시 한 번 만들어 보세요.
  - 어떤 컴포넌트가 필요한지 생각해 보세요.
  - 각 컴포넌트별로 해당하는 CSS와 자바스크립트를 어떤 식으로 붙여야 할까요?
  - Vue.js 시스템에 타입스크립트는 어떤 식으로 적용할 수 있을까요?
  - 컴포넌트간에 데이터를 주고받으려면 어떤 식으로 하는 것이 좋을까요?
  - `vue-cli`와 같은 Vue의 Boilterplating 기능을 이용하셔서 세팅하시면 됩니다.

## Advanced

- React와 Angular는 어떤 프레임워크이고 어떤 특징을 가지고 있을까요? Vue와는 어떤 점이 다를까요?
- Web Component는 어떤 개념인가요? 이 개념이 Vue나 React를 대체하게 될까요?
- Reactive Programming이란 무엇일까요?
