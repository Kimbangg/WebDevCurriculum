# CheckList

## 1. CSS를 HTML에 적용하는 세 가지 방법은 무엇일까요?

 <br/>

- ### 인라인(inline Style Sheet)

  - HTML 태그의 style 속성에 css 코드를 삽입하는 방법입니다.
    <br>

  ```
    <p style="color: blue">Lorem ipsum dolor.</p>
  ```

- ### Internal Style Sheet

  - HTML 파일 > `<style></style>` 안에 css 코드를 작성하는 방법입니다.

    ```
    // in index.html

    <style>
      h1 {
        color: red;
      }
    </style>
    ```

- ### Linking Style Sheet
  - 별도의 css 파일을 만들고, HTML 문서와 연결하는 방법입니다.
    ```
    // in index.html
    <link rel="stylesheet" href="style.css">
    ```

<br>

## 2. CSS 규칙의 우선순위는 어떻게 결정될까요?

1. 속성 값 뒤에 !important 를 붙인 속성
2. 인라인으로 style 속성 지정
3. #id 로 지정한 속성
4. .클래스, :추상클래스 로 지정한 속성
5. 태그이름 으로 지정한 속성

<br>

## 3. CSS 박스모델은 무엇일까요? 박스가 화면에 차지하는 크기는 어떻게 결정될까요?

- 모든 HTML 요소들은 박스(Box)모양으로 구성되며, 이 것을 `박스 모델` 이라고 합니다.
- 박스모델은 HTML 요소를 패딩(Padding), 테두리(border), 마진(margin), 내용(Content)로 구분합니다.
  <br><br>
  - 각 영역들을 상세하게 알아보자!
    - 콘텐츠 영역
      - 콘텐츠는 속성의 기본 값이 box-sizing이며 width, height로 크기 조정이 가능합니다.
    - 마진(Margin)
      - 마진은 테두리와 이웃하는 요소 사이의 간격입니다.
    - 패딩(Padding)
      - 박스모델의 테두리와 컨텐츠 사이의 간격입니다.

<br>

## 4. float 속성은 왜 좋지 않을까요?

- float은 부모 요소가 자식 요소의 크기를 반영하지 못하고, 상속되는 문제가 발생합니다.

<br>

## 5. Flex와 Grid의 차이

- flex는 1차원 수평, 수직 영역 중 하나의 방향으로만 레이아웃을 나눌 수 있습니다.
- grid는 2차원 수평, 수직을 동시에 나눌 수 있습니다.
  <br><br><br>

# Advanced

## 1. 왜 css는 어려울까요?

- 중복 선언된 규칙 중에 어떤 값이 적용될지 예측하기가 어렵다.
- 어디서든 선언이 될 수 있기 때문에, 네이밍과 통제가 어렵다.
- 다른 속성과의 상호작용을 예측하기가 어렵다.

<br>

## 2. css의 어려움을 극복하기 위해서 사용되는 방법

- 중복 선언을 피하기 위해서, BEM 방식을 통해서 사용처를 명확히한다.
- Module 화를 통하여 중복 방지 및 통제를 편리하게 한다.

<br>

## 3. css가 브라우저에 의해 해석되고 적용되기까지 내부적으로 어떤 과정을 거칠까요?

<br>

### 1. Critical Rendering Path

- css를 바탕으로 cssom Tree를 생성한다.
- Dom 트리와 CSSOM Tree를 합쳐서 렌더링 트리가 생성된다.
  - Dom트리를 순회하면서, CSSOM 데이터를 추가했을 때 렌더트리가 생성된다.
- Render Tree에서 제공한 위치 값을 가지고 Layout을 설정한다.
- 레이아웃이 완료 되었다면, 실제 이미지와 색 등을 삽입하는 Paint가 진행된다.

### 2. UI가 업데이트되는 3가지 상황

- Layout이 다시 발생되는 경우
  - 요소이 크기나 위치 또는 브라우저의 창이 바뀌는 레이아웃이 변경될 때
- Paint부터 다시 발생되는 경우
  - 배경 이미지나 텍스트 색상 등 레이아웃의 수치를 변경시키지 않는 변경이 일어났을 때
- 레이어의 합성만 다시 발생하는 경우
