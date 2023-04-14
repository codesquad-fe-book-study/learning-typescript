# 7장 인터페이스

- 객체의 형태를 설명하는 방식

## 1. 인터페이스

- 왜 타입이 아니라 인터페이스를 사용할까?
    - 여러 인터페이스의 property를 합쳐서 병합해서 사용할 수 있다.
    - 클래스가 선언된 구조의 타입을 확인할 수 있다.
    - 타입 검사기가 더 빠르게 작동한다. (How? 내부적으로 더 쉽게 캐시할 수 있는 명명된 타입을 선언한다.)
    - 오류 메시지를 쉽게 읽을 수 있다.
- 객체 형태의 경우 유니언 타입과 같은 기능이 필요할 때만 타입 별칭으로 선언하고, 아니면 인터페이스로 사용한다.

## 2. 속성 타입

### 2.1 선택적 속성 `?:`

### 2.2 읽기 전용 프로퍼티 readonly

- 코드 영역에서 객체를 의도치 않게 수정하는 것을 막아준다.

### 2.3 함수와 메서드

- readonly로 선언해야 하는 경우 → 속성으로 선언하기

### 2.4 Call Signature

- 함수의 형태를 미리 선언하는 방식

```ts
type FunctionAlias = (input: string) => number
const typeFunctionAlias: FunctionAlias = (input) => input.length

interface CallSignature {
	(input: string): number // 이 프로퍼티가 함수라는 의미
}
const typedCallSignature: CallSignature = (input) => input.length;

type Add = (a:number, b:number) => number
const plus:Add = (a, b) => a + b
```

### 2.5 인덱스 시그니처

- 객체가 가진 임의의 키와 프로퍼티를 나타내는 구문

```tsx
interface WordCounts {
	[i: string]: number;
}

// string 타입의 임의의 키를 가지고, value의 타입은 number이다.
```

- 숫자 인덱스 시그니처: key에 string 대신 number type 사용 가능

### 2.6 중첩 인터페이스

- 인터페이스 안에 인터페이스, 객체를 가질 수 있다.

## 3. 인터페이스 확장

1. **타입이 다른 동일한 이름의 속성은 여러 번 선언할 수 없다.**
2. 동일한 이름과 다른 시그니처를 가진 메서드는 가능하다. 함수 오버로드 발생

```ts
interface MergedProperties {
	different: (input: string) => string;
}

interface MergedProperties {
	different: (input: number) => string;
}

interface MergedMethods {
	different(input: string): string;
}

interface MergedMethods {
	different(input: number): string;
}
```
