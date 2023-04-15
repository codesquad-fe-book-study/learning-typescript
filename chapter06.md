# 6장 배열

### 배열 멤버

```jsx
const defenders = ["Clarenza", "Dina"]

// defenders 타입은 string이기 때문에 -> defender 타입도 string
const defender = defenders[0];
```

```jsx
const soldiersOrDates = ["han", new Date()]

// 타입: string | Date
const soldierOrDate = soldiersOrDates[0];
```

**주의사항**

인덱싱 연산자(’[]’)를 사용해서 배열 또는 객체의 속성에 접근할 때, 해당 속성이 존재하지 않는 경우 컴파일러가 경고하지 않는다.

**이유는?**

<img width="688" alt="스크린샷 2023-04-09 오후 12 04 10" src="https://user-images.githubusercontent.com/81420856/230751966-d96fb52b-60a3-4a8d-b43a-8eef370434b7.png">

## 6.4 튜플

### 튜플 할당 가능성

```jsx
const pairLoose = [false, 123];
// [boolean, number] 형태지만 타입은: (boolean|number)[]
const pairTupleLoose : [boolean, number] = pairLoose
// Error
```

타입스크립트는 튜플 타입의 멤버의 수를 알고 있다.

그렇기 때문에 멤버의 수가 2인 타입 `[boolean, number]`에 타입이 `(boolean|number)[]` 인 값을 할당하는 것은 불가능하다.

다만 그 반대의 경우는 가능하다

```jsx
let tuple : [boolean, number]
tuple = [true, 1];

let optional: (boolean | number)[]
optional = tuple;
// ok
```

### 튜플 추론

생성된 배열은 튜플이 아닌 가변 길이의 배열로 취급한다.

```jsx
function charAndLength(input: string){
	return [input[0], input.length]
}

const [first, second] = charAndLength("hello");
// first 타입은 strings, second 타입은 number일 것 같지만 아니다.
// first 타입: string|number
// second 타입: string|number
```

charAndLength의 타입을 배열 타입 대신 구체적인 튜플 타입으로 하고 싶으면 두가지 방법이 있다. 명시적 튜플 타입 or const 어서션

**명시적 튜플 타입**

```jsx
function charAndLength(input: string): [string, number]{
	return [input[0], input.length];
}

const [first, second] = charAndLength("hello");
// first 타입: string
// second 타입: number
```

**const 어서션**