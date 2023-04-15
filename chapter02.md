# 2장 타입 시스텝

### 타입 시스템

타입 스크립트는 자바스크립트와 마찬가지로 기본적인 타입은 js의 원시 타입과 동일하다.

근데 bigint와 symbol 두 가지는 사용을 안하다보니 좀 생소하다. 그래서 잠깐 bigint에 대해서 mdn에서 알아보니, bigint는 ‘2^53-1보다 큰 정수를 표현할 수 있는 내장객체’ 라고 설명되어 있다.

특징은, bigint는 Math 객체의 메서드와 함께 사용할 수 없고, 연산에서 Number와 혼합해 사용할 수 없다.

```jsx
typeof BigInt('1') 
=== 'bigint'; // true
0n === 0 // false
0n == 0 // true
0n === Object(0n); // false
Object(0n) === Object(0n); // false

const o = Object(0n);
o === o // true
```

| js | ts |
| --- | --- |
| null | null |
| undefined | undefind  |
| boolean | true → boolean |
| string | string |
| number | 1234 |
| bigint | 1337n |
| symbol | Symbol(’Franklin”) |

그리고 시험삼아 typescript 플레이그라운드에 bigint 타입을 적어보니 다음과 같은 오류가 떴다.

```tsx
let bigint : bigint = 0n
//BigInt literals are not available when targeting lower than ES2020.
```

찾아보니, bigint 리터럴은 ES2020 이상의 버전을 지원하는 환경에서만 사용할 수 있어서 뜬 오류였다.

아무튼 그래서, ts는 js와 동일한 7가지 원시 타입이 있는데, 사용자가 변수의 타입을 선언하지 않고 변수에 초기값인 데이터를 할당하면, ts는 사용자에게 할당된 데이터를 바탕으로 변수의 타입을 7가지 원시 타입 중 하나로 유추해서 사용자에게 알려준다. 그리고 초기값을 바탕으로 bestSong 변수가 허용되는 타입을 결정한다.

```tsx
let bestSong = Math.random() > 0.5
	? 5
	: "Respect"
// 변수에 mousehover 시 bestSong 변수가 String 또는 Number 임을 알려준다.
```

근데 여기서 한가지 의문점이 있다. 변수 bestSong는 true일 때 숫자인 5가 반환되고, false시 문자열인 “Restect”가 반환되서, 변수에 hover시

`let bestSong: number | string` 을 보여줄 것 같은데,

`let bestSong: string | number`  을 보여준다. 

([https://www.typescriptlang.org/ko/play](https://www.typescriptlang.org/ko/play) 기준이다)

왜 일까? 찾아보니 별다른 의미는 없고, 둘 다 같은 의미고 순서에 규칙은 없다고 한다. TS에서 **`string | number`**와 **`number | string`**은 같은 타입이고, 순서는 의미가 없는 유니온 타입(Union Type)이다. 

코드 가독성을 고려해서 유니온 타입의 순서를 일관된 순서로 작성하는 것이 좋다는데, 코드 구현시 유니온 타입의 순서를 일관되게 작성하면 가독성을 높일 수 있다는 것 같다. 

이유를 찾고 나서 보니, 위에서 타입스크립트 플레이그라운드에서 저렇게 보여 준 이유는 아마도 저 사이트 개발자 분이 저렇게 정해서 그런거 일지도 모른다는 추측이 든다. 근데 왜지…? 가독성 상관없이 순서도 연산자 순서에 맞게 타입을 알려주면 더 좋을 것 같다는 생각을 하게 된다.

### 타입 애너테이션

변수에 초기값이 없으면 TS는 초기 타입을 파악하지 않고 해당 변수를 any 타입으로 간주한다. any 타입은 ‘any’라는 단어로도 유추할 수 있듯이, 어떤 타입도 될 수 있다. 이는 TS의 타입 검사 기능을 부분적으로 쓸모없게 만들 수도 있다.

따라서 이 책의 저자가 "보통 **`any`** 타입을 사용하여 **`any`** 타입으로 진화하는 것을 허용하게 되면 TS의 타입 검사 기능을 부분적으로 쓸모없게 만들 수 있다"고 말한게 어느정도 이해가 된다.

예를 들어, **`function human(job: any){}`** 함수의 매개변수 타입을 **`any`**로 지정한다면, 이는 1.2장에서 언급된 JS의 함정 중 하나인 "값 비싼 자유"를 TS에서 되풀이 할 수도 있겠다는 생각이 든다.

따라서 TypeScript 코드를 작성할 때는 **`any`** 타입을 최대한 피하고, 타입 추론을 사용하거나 타입을 명시적으로 지정하여 코드의 가독성과 유지보수성을 높이는 것이 좋을 것 같다.

### 타입 형태 - 모듈

TS는 export와 import 구문을 표준화하기 위해 나온 ES6 모듈 시스템을 지원한다. 하지만 CommonJS와 같은 이전 모듈은 지원하지 않는다. 왜 일까?

찾아보니 이전 모듈 시스템은 ES6 모듈 시스템보다 유연성과 성능면에서 떨어지기 때문에 TS는 기본적으로 ES6 모듈 시스템을 사용한다고 한다.

그렇다고 아예 사용 못하는건 아니고, 만약 TS로 CommonJs을 사용하고 싶으면 ‘tsconfig.json’파일에서 ‘module’ 옵션을 ‘commonjs’로 설정하면 된다.