# Quest 04. OOP의 기본

## Introduction

- 이번 퀘스트에서는 바닐라 자바스크립트의 객체지향 프로그래밍에 대해 알아볼 예정입니다.

## Topics

- 객체지향 프로그래밍
  - 프로토타입 기반 객체지향 프로그래밍
  - 자바스크립트 클래스
    - 생성자
    - 멤버 함수
    - 멤버 변수
  - 정보의 은폐
  - 다형성
- 코드의 재사용

## Resources

- [MDN - Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
- [MDN - Inheritance and the prototype chain](https://developer.mozilla.org/ko/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [MDN - Inheritance](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Inheritance)
- [Polymorphism](https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-3-polymorphism-fb564c9f1ce8)
- [Class Composition](https://alligator.io/js/class-composition/)
- [Inheritance vs Composition](https://woowacourse.github.io/javable/post/2020-05-18-inheritance-vs-composition/)

## Checklist

- 객체지향 프로그래밍은 무엇일까요?
  - 기능이 아닌 객체가 중심이 되며 누가 어떤 일을 할 것인가?가 핵심이 된다.<br>
  - 이 때, 객체는 Instance로 만들어지기 전까지는 실행이 되지 않는다.
  - 미국인들이 추상적인 어떤 것을 이야기 할 때 관사를 제외하고 부르는데 그 것을 객체, 그리고 특정 물체를 말할 때 관사를 붙이는 데 그 것을 인스턴스라고 생각하면 된다.
    <br><br>
- `#`로 시작하는 프라이빗 필드는 왜 필요한 것일까요? 정보를 은폐(encapsulation)하면 어떤 장점이 있을까요?

  - #는 private 필드로, 클래스 외부에서는 읽거나, 변경할 수 없습니다.
  - 직원들에게 월급을 주는 프로그램이 있다고 가정을 해봅시다. 이 때, 누군가 이 프로그램을 조작하여 특정 직원의 월급을 높게 측정을 해둘 수 있다면 어떤 일이 일어날까요?
  - 객체지향에서는 이런 일을 막기 위해서, 중요한 데이터의 경우 은닉화하여 외부에서는 직접 접근 할 수 없도록 막습니다. (Company.Empolyee.money = 50000000 X)

  <br>

- 다형성이란 무엇인가요? 다형성은 어떻게 코드 구조의 정리를 도와주나요?

  - 하나의 타입에 여러 개의 객체를 대입할 수 있는 성질을 의미합니다.
  - 다형성은 보통 상속을 통해서, 구현 할 수 있습니다.

    ```
    public static void main(String[] args) {
      // figure에 triangle객체 대입 => 다형성
      Figure figure = new Triangle(3, 10);

      // 다형성이 없었다면?
      if (dot == 3) {
        Triangle triangle = new Triangle(3, 10);
        for (int i = 0; i < triangle.getDot(); i++) {
            triangle.display();
        }
      } else if(dot == 4) {
        Rectangle rectangle = new Rectangle(4, 20);
        for (int i = 0; i < rectangle.getDot(); i++) {
            rectangle.display();
        }
      }

    }
    ```

    <br>

- 상속이란 무엇인가요? 상속을 할 때의 장점과 단점은 무엇인가요? (is-a)
  - super()는 부모의 생성자 함수를 호출합니다.
    <br>
  - 장점
    - 객체의 재사용성이 높아집니다.
    - 코드의 간결성을 제공합니다.
    - 유지/보수가 간편해집니다.
  - 단점
    - 부모, 자식 클래스 간의 의존성 & 결합도가 높습니다.
    - 상속 구조가 복잡해지면 하위 구조에 미치는 영향을 예측하기가 어렵다.
    - 적절하지 못한 상속이 되면, 의도했던 것과 다르게 동작합니다.
      <br><br>
- OOP의 합성(Composition)이란 무엇인가요? 합성이 상속에 비해 가지는 장점은 무엇일까요? (has-a)

  - 상속으로 프로그램을 짜는 경우, 다양성이 증가하면 이를 관리하기가 어려워진다는 문제가 발생할 수 있다.
  - 그렇기 때문에, 재사용이 되는 특정 클래스를 별도로 만들어두고 이를 합성해서 사용하는 방식을 통해 불필요한 하위 클래스 생성을 최소화 할 수 있다.
    <br><br>

  ```
    public class FastFood implements Eatable {
      private Burger burger;
      private Beverage beverage;

      public FastFood(Burger burger, Beverage beverage) {
          this.burger = burger;
          this.beverage = beverage;
      }

      public void setBurger(Burger burger) {
          this.burger = burger;
      }

      public void setBeverage(Beverage targetBeverage) {
          this.beverage = targetBeverage;
      }

      ...
  }
  ```

## Quest

- 웹 상에서 동작하는 간단한 바탕화면 시스템을 만들 예정입니다.
- 요구사항은 다음과 같습니다:
  - 아이콘은 폴더와 일반 아이콘, 두 가지의 종류가 있습니다.
  - 아이콘들을 드래그를 통해 움직일 수 있어야 합니다.
  - 폴더 아이콘은 더블클릭하면 해당 폴더가 창으로 열리며, 열린 폴더의 창 역시 드래그를 통해 움직일 수 있어야 합니다.
  - 바탕화면의 생성자를 통해 처음에 생겨날 아이콘과 폴더의 개수를 받을 수 있습니다.
  - 여러 개의 바탕화면을 각각 다른 DOM 엘리먼트에서 동시에 운영할 수 있습니다.
  - Drag & Drop API를 사용하지 말고, 실제 마우스 이벤트(mouseover, mousedown, mouseout 등)를 사용하여 구현해 보세요!

## Advanced (x)

- 객체지향의 역사는 어떻게 될까요?
- Smalltalk, Java, Go, Kotlin 등의 언어들로 넘어오면서 객체지향 패러다임 측면에서 어떤 발전이 있었을까요?
