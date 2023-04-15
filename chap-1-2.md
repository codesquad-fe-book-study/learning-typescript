# Chap 1 ~ 2

# 1장. 자바스크립트에서 타입스크립트로

## 자바스크립트

- 다른 언어는 컴파일러가 충돌할 수 있다고 판단하면 코드 실행을 거부할 수 있다. 하지만 자바스크립트는 `동적 타입 언어` 로 충돌 가능성을 먼저 확인하지 않고 코드를 실행한다. 이런 자바스크립트의 자유는 재미를 주기도 하지만 안정성면에서는 위험 부담이 있다.
- JSDoc: 자바스크립트 소스 코드에 주석을 달기 위해 사용하는 마크업 언어

```jsx
/**
 * Performs a pinater painting
 *
 * @param {Painting} painter
 * @param {string} painting
 * @returns {boolean} Whether the painter painted the painting.
 */
function paintPainting(painter, painting) {//...}
```

타입스크립트의 타입을 주듯이, JSDoc을 작성해두면 타입에 대한 힌트를 제공받을 수 있다.

그러나

1. 잘못된 코드를 막을 수는 없다는 점
2. 리팩터링 중 생긴 변경사항과 관련된 현재는 유효하지 않은 JSDoc 주석을 모두 찾는 건 어렵다.
3. 다소 복잡한 객체를 설명하기 어렵다.

[https://poiemaweb.com/jsdoc-type-hint](https://poiemaweb.com/jsdoc-type-hint)

## 타입스크립트

- 자바스크립트의 상위 집합(`superset`) 또는 타입이 있는 자바스크립트
- 구성
  - 프로그래밍 언어: 자바스크립트에 타입과 관련된 고유 구문이 포함
  - 타입 검사기: JS, TS로 작성된 파일에서 생성된 모든 구성 요소를 이해하고 잘못된 부분을 알려주는 프로그램
  - 컴파일러: 타입 검사기를 실행하고 발생한 문제를 알려준 뒤, 이에 대응되는 JS 코드를 생성하는 프로그램
  - 언어 서비스: 타입 검사기를 사용하여 편집기(VScode, 웹스톰 등)에 개발자에게 유용한 api 제공법을 알려주는 프로그램
- 제한을 통해 더 자유로워질 수 있다.
- 코드를 통한 코드의 문서화 ex) interface

    ```tsx
    interface Example {
    	name: string;
    	isOkay(): boolean;
    	paint(painting: string, materials: Material[]): boolean;
    }
    ```

  위와 같이 interface를 선언하는 것만으로도 Example을 구현하는 class에 어떤 프로퍼티, 메서드가 있을지 알 수 있다.

- 타입스크립트를 사용하면 편집기를 더 똑똑하게 만들 수 있다.
- 타입스크립트 컴파일 자체는 검사와는 별도로 언제나 JS 코드를 내보낸다. 그러나 대부분 타입스크립트 자체 컴파일러보단 바벨과 같은 번들러로 번들링할 때 함께 진행한다.
- 타입스크립트를 사용하면 클래스를 꼭 사용해야한다거나, 객체지향을 써야한다거나 어떤 코드 스타일 의견을 강요하지 않는다.
- 타입스크립트는 자바스크립트의 작동 방식에 전혀 영향을 주지 않는다.
- 브라우저난 Node.js와 같은 런타임 환경에서 실행할 수 있는 코드는 결국엔 JS 코드이다.
  - ts-node, 디노가 TS 코드를 바로 실행시키는 것처럼 보여도 실제론 내부적으로 자바스크립트로 변환한다.

# 2장. 타입  시스템

- 타입 검사기는 실제로 어떻게 작동할까??

## 타입의 종류

- 타입: 자바스크립트에서 다루는 값의 `형태` 에 대한 설명
- 기본적으로 어떤 변수에 할당하는 값을 통해 타입을 추론한다.

```tsx
let a = null; // null
let b = undefined; // undefined
let c = true; // boolean
let d = 'TypeScript'; // string
let e = 1116; // number
let f = 1116n; // bigint
let g = Symbol('Jayden'); // symbol
```

### 타입 시스템

- 프로그래밍 언어가 프로그램에서 가질 수 있는 타입을 이해하는 방법에 대한 규칙

### 오류의 종류

- 구문 오류: 타입스크립트가 자바스크립트로 변환되는 것을 차단하는 경우
- 타입 오류: 타입 검사기에 따라 일치하지 않는 것이 감지된 경우
  - 오류가 발생해도 자바스크립트 코드로 변환은 일어난다.

## 할당 가능성

- 타입스크립트에서 함수 호출이나 변수에 값을 제공할 수 있는지 여부를 확인하는 것

```tsx
let count = 10;
count = 'hodu'; // Type... is not assignable to type
```

## 타입 애너테이션

- 타입 주석 정도로 해석하면 된다. 변수에 값을 할당하지 않고도 타입을 부여할 수 있다.

```tsx
let a; // any
a = 1;
a = 'j';

// type annotation
let b: string;
b = 'j';
b = 2; // error
```

- 타입 애너테이션 중복
  - 가급적이면 아무것도 변하지 않는 변수에는 타입 애너테이션을 추가하지 않는다.

```tsx
let a: string = 'jjj';
```

## 타입 형태

- 타입스크립트는 타입을 부여한 객체에 어떤 멤버 프로퍼티, 메서드가 있는지 알고 있다.

### 모듈

- ES6에 추가된 `import, export`
- 타입스크립트는 파일을 기본적으로 전역 스코프로 간주한다.
- 같은 디렉토리에 a.ts, b.ts가 있을 때,

```tsx
// a.ts
const name = 'jayden';

// b.ts
const name = 'hodu'; // 에러 발생
```

해결법

1. 파일에 `export {};` 해주기
2. 파일 소스코드 전체를 `{}` 으로 감싸서 block-scope를 나눠주기
3. ts 파일의 확장자를 `mts`로 한다.(모듈임을 알리는 것)

추가) 타입스크립트는 일반적으로 CommonJS 스타일의 require 함수에서 반환된 값을 any로 인식한다.