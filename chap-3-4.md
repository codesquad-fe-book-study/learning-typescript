# 3장. 유니언과 리터럴

- 유니언(union): 값에 허용된 타입을 2개 이상의 가능한 타입으로 확장
- 내로잉(narrowing): 값에 허용된 타입을 하나 이상의 가능한 타입이 되지 않도록 좁히기

⇒ 다른 프로그래밍 언어에서는 불가능하지만 TS에서는 **가능한 코드 정보에 입각한 추론**

## 유니언

```tsx
let example = Math.random() > 0.5 ? 'j' : 1116;
// example: string | number
```

**주의할 점**

```tsx
example.toUpperCase(); // Error
// toUpperCase라는 메서드는 number type에 없기 때문이다.
// 즉, example에는 number type인 값이 할당될 수 있기 때문에 이에 대해서 안전장치를 해주는 것
```

위의 예시를 보면 **유니언은 마치 ‘또는’(||) 같지만 객체의 개념에서는 한쪽이 아닌 모두 갖고 있는 프로퍼티, 메서드만 사용할 수 있기에 약간 && 느낌이 난다.**

⇒ string | number을 객체의 개념으로 생각하면 이해가 되는 것 같다!!!

```tsx
let test: string | number;

이렇게 되면 test에는 주로 string, number 리터럴 값이 할당될텐데, 얘네는 모두 각각 어떤 프로퍼티,
메서드를 갖고 있다. 그렇게 생각하면 동시에 갖는 속성이 아니면 할당이 안되는 게 이해된다.
```

```tsx
type exam1 = {name: string, age: number}
type exam2 = {name: string, agee: number}

let test: exam1 | exam2;
test = {name: 'j', age: 1} // 가능
test = {name: 'l', agee: 2} // 가능
test = {name: 'j', age: 1, agee: 2} // 가능
```

## 내로잉

말 그대로 타입을 좁힌다. 이 때, 타입 내로잉과 조건문을 함께 씀으로써 `타입 가드`의 역할을 하게 할 수 있다.

위 말이 책에서 말하는 타입가드: **“타입을 좁히는 데 사용할 수 있는 논리적 검사”**

```tsx
let example = Math.random() > 0.5 ? 'j' : 1116;
// example: string | number

if (example === 'j') {
	example.toUpperCase() // string으로 타입이 좁혀졌기 때문에 가능
} else {
	example.toFixed() // error 발생.
}

---

if (example === 'j') {
	example.toUpperCase() // string으로 타입이 좁혀졌기 때문에 가능
} else if (example === 1116) {
	example.toFixed() // 이건 확실히 1116으로 명시했기에 가능
}

---

if (example === 'j') {
	example.toUpperCase() // string으로 타입이 좁혀졌기 때문에 가능
} else if (example === 555) {
	example.toFixed() // 이건 확실히 1116으로 명시했기에 가능
}

---

if (typeof example === 'string') { // 조건을 이렇게 주면 가능
	example.toUpperCase()
} else {
	example.toFixed() 
}
```

## 리터럴 타입

- 좀더 구체적인 버전의 원시 타입

```tsx
let age = 123;

const name = 'jayden';
```

위와 같이 `const`로 변수를 선언 및 할당하는 경우 그 값 자체가 타입이 된다.(리터럴 타입)

⇒ 어차피 const로 선언된 변수는 값이 변경 혹은 초기화될 일이 없으니까

그래서 `원시 타입`은 해당 타입의 가능한 모든 `리터럴 값의 집합`인 것이다.

## 엄격한 null 검사

**strictNullChecks: true - null 할당 안됨 / false - null 할당 가능 ⇒ 사실상 무조건 true를 하는 게 좋다.**

```tsx
const test = Math.random() > 0.5 ? 'j' : undefined;
let t = test && test.toUpperCase(); 
// 가능( && 연산자를 통해 test truthy, falsy로 걸러지기 때문이다. )

---

let p = test?.toUpperCase();
// 가능( ? 연산자 자체가 test가 undefined일 가능성을 말해주는 것이기 때문이다. )
```

## 타입 별칭(Type alias)

**타입을 변수처럼 선언하는 것**

# 4장. 객체

**자바스크립트는 결국 모두 객체!!! null, undefined를 제외한 모든 값은 그 값에 대한 실제 타입의 멤버(프로퍼티, 메서드)를 가지므로 모든 값의 타입을 확인하기 위해서는 객체 타입을 이해해야한다.**

## 구조적 타이핑

TS의 타입 시스템은 `구조적으로 타입화`되어있다. 타입 검사기에서 정적 시스템이 타입을 검사한다.

반면 JS는 `덕 타이핑(덕 타입)` 으로 런타임에서 사용될 때까지 객체 타입을 검사하지 않는다.(동적 타이핑)

### 초과 속성 검사

```tsx
type A = {name: string}

type B = {name: string, age: number}

const t = {name: 'j', age: 11}

let ta: A = t; // 에러 안남 - 초과 속성 검사 피해가기

let tb: A = {name: 'l', age: 12} // 에러 발생

let tc: B = {name: 'l'} // 이건 당연히 에러
```

초과 속성 검사는 객체 타입으로 선언된 위치에서 생성되는 객체 리터럴에 대해서만 일어난다.

## 객체 타입 유니언

리터럴 타입이나 원시 타입과 마찬가지로 모든 객체 타입에 존재하지 않는 속성에 접근하기 위해서는 타입을 좁혀야 한다.(`타입 가드`)

```tsx
const person = Math.random() > 0.5 
									? {name: 'j', age: 123} 
									: {name: 'l', phoneNumber: 10}

person.name // 가능
person.age // 불가능(person.age가 number | undefined이기 때문이다.)
person.phoneNumber // 불가능(person.phoneNumber가 number | undefined이기 때문이다.)
```

## 객체 타입 내로잉

```tsx
if ('age' in person) {
	person.age; // 가능
} else {
	person.phoneNumber // 가능
}

---

// 아래와 같이 사용하지 않는다는 거 기억하기!!!
if (person.age) {
	person.age; 
} else {
	person.phoneNumber 
}
```

## 교차 타입

```tsx
type A = {name: string, age: number};
type B = {name: string, phoneNumber: number};

type C = A & B;
// {name: string, age: number, phoneNumber: number}
```

## Never 타입

```tsx
let t: string & number;

// t: nerver => 불가능한 타입(존재할 수 없는 타입)
// 거의 사용하지 않지만, 불가능한 상태를 나타내기 위해 가끔씩 등장한다.
```

```ts
function hello(name:string|number) {
  if (typeof name === "string") {
    name // string
  } else if (typeof name === "number") {
    name // number
  } else {
    name // never
  }
}
```