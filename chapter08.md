# 클래스

## 8.2.2 초기화 검사

- 엄격한 컴파일러 설정이 활성화된 상태 -> 타입스크립트는 undefined 타입으로 선언된 각 속성이 생성자에서 할당되었는지 확인한다.

```js
// 문제!
class Parent {
  constructor(number: number){
    this.number = number; // 에러 여부
  }
}

class WithValue extends Parent {
  person = "jayden"
  number: number // 에러 여부

  constructor(number: number) {
    super(number);
  }
}

const withValue = new WithValue(0);
console.log(withValue.number) // 결과?
```

## 번외 문제

```js
class Human {
  constructor(){
    this.sayHello()
  }
}

class Jayden extends Human {
  constructor(){
    super();
    this.hello = "hello"
  }

  sayHello(){
    console.log(this.hello);
  }
}

const jayden = new Jayden();
// 실행하면 어떤 값이 나올까요?
```

