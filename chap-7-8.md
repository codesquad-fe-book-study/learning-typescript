# 7장. 인터페이스

- 인터페이스는 `객체 형태를 설명하는 또 다른 방법`
- types alias로 정의된 객체 타입과 유사하지만 차별점 존재
  - 더 읽기 쉬운 오류 메시지
  - 더 빠른 컴파일러 성능
  - 클래스와의 더 나은 상호 운용성

## Type alias VS Interface

두 가지 모두 세미콜론과 쉼표 둘 다 가능하다.

인터페이스의 특징

- 속성 증가를 위해 병합이 가능하다.
- 클래스가 선언된 구조의 타입을 확인하는데 사용 가능하다.
- 일반적으로 인터페이스에서 타입 검사기가 더 빨리 작동한다. 내부적으로 더 쉽게 캐시할 수 있는 명명된 타입을 선언하기 때문이다.
- 이름 없는 객체 리터럴의 별칭이 아닌 이름이 있는 하나의 객체로 간주되므로 오류 메시지를 좀더 쉽게 읽을 수 있다.

## 읽기 전용 속성(readonly)

- 인터페이스에 정의된 객체의 속성을 재할당하지 못하도록 차단할 때, readonly 키워드를 추가한다.
- readonly 연산자는 타입 시스템에만 존재하며 인터페이스에서만 사용할 수 있다. 단지, 개발 중에 그 속성이 수정되지 못하도록 보호한다.

## 메서드와 속성 함수

- 메서드 구문: `test(): void;`
- 속성 함수 구문: `test: () => void;`

> 메서드는 readonly 선언이 불가능, 속성 함수는 가능

## 호출 시그니처

함수 타입과 유사하지만, 콜론(:)이 아닌 화살표(=>)로 표시한다.

```ts
interface TestImpl {
  (input: string): number;
}

const test: TestImpl = (input: string) => input.length;
```

## 인덱스 시그니처

- 자바스크립트 객체 속성 조회는 암묵적으로 키를 문자열로 변환하기 때문에 인터페이스의 객체는 일반적으로 문자열 키를 사용한다.

```ts
interface Example {
  [i: string]: number;
}
```

- 인덱스 시그니처는 객체에 값을 할당할 때 편리하지만 `타입 안정성을 완벽하게 보장하지는 못한다.`
- 명명된 속성이 더 구체적인 타입을 제공하고, 다른 모든 속성은 인덱스 시그니처의 타입을 대체하는 것으로 혼합해서 사용할 수 있다.
- 일반적으로는 인덱스 시그니처의 원시 속성보다 더 구체적인 속성 타입 리터럴을 사용한다.

```ts
interface Dog {
  name: 'hodu'; // 더 구체적인 리터럴
  [i: string]: string;
}
```

## 인터페이스 확장

- 인터페이스가 다른 인터페이스의 모든 멤버를 복사해서 선언할 수 있는 확장된 인터페이스
- 확장할 인터페이스의 이름 뒤에 `extends` 키워드를 추가한다.

### 재정의된 속성

- 타입 검사기는 재정의된 속성이 기본 속성에 할당되어있도록 강요한다.
- 즉, 재정의한 속성은 이전 정의보다 더 좁은 범위여야 한다.

### 다중 인터페이스 확장

- 인터페이스는 여러 개의 인터페이스를 확장해서 선언할 수 있다.
- extends 키워드 뒤에 쉼표로 인터페이스 이름을 구분해 사용하면 된다.

## 인터페이스 병합

- 여러 개의 인터페이스가 동일한 이름으로 동일한 스코프에 선언된 경우, 선언된 모든 필드를 포함하는 더 큰 인터페이스가 된다.
- 인터페이스 병합을 자주 사용하지는 않는다.
- 인터페이스 병합은 외부 패키지 또는 Window와 같은 내장된 전역 인터페이스를 보강하는데 특히 유용하다.

### 이름이 충돌되는 멤버

- 속성이 이미 인터페이스에 선언되어 있다면 나중에 병합된 인터페이스에서도 동일한 동일한 타입을 사용해야 한다.
- 같은 이름의 속성이 타입이 서로 다르면 오류가 발생한다.
- 다만, 동일한 이름과 다른 시그니처를 가진 메서드는 정의할 수 있다.(함수 오버로드)

# 8. 클래스

명심할 내용: `타입스크립트는 클래스 사용이나 다른 인기 있는 자바스크립트 패턴을 권장하지도 막지도 않는다.`

> 개인적인 생각: 그렇지만 자바 언어가 클래스로 객체를 표현하는 객체 지향 언어인만큼 타입스크립트도 클래스로 구현을 많이 하는 것 같다.

## 클래스 속성

- 타입스크립트에서 클래스의 속성을 읽거나 쓰려면 클래스에 명시적으로 선언해야한다.

```ts
class Example {
  test: string; // 이런 식으로 속성의 타입을 명시적으로 선언해야된다.
  constructor() {
    this.test = 'ttt';
  }
}
```

> `타입스크립트는 생섲아 내의 할당에 대해서 그 멤버가 클래스에 손재하는 멤버인지 추론하려고 시도하지 않습니다.`
> 즉, 위처럼 명시를 해주지 않으면 TS가 스스로 추론하지 못하는 것을 의미한다.

## 함수 속성

```ts
class T {
	arrow: () => void;
	constructor() {
		this.arrow = () => {
			console.log(this)
		}
	}

	nonArrow() {
		console.log(this)
	}
}

const t = new T();
t.arrow(); // 'T: {}'가 찍힌다.
t.nonArrow(); // 'T: {}'가 찍힌다.
t.arrow.bind('arrow입니다.')(); // 'T: {}'가 찍힌다.
t.nonArrow.bind('메서드입니다.')(); // '메서드입니다.'가 찍힌다.
```

```ts
class T {
  t = 111;
}
// 위와 아래는 같은 표현이다.
class T {
  t: number;
  constructor() {
    this.t = 111
  }
}
```

## 초기화 검사

- 엄격한 컴파일러 설정이 활성화된 상태에서는 타입스크립트가 undefined 타입으로 선언된 각 속성이 생성자에서 할당되었는지 확인한다.

> 엄격한 초기화 검사를 적용하면 안되는 속성인 경우에는 이름 뒤에서 `!`를 추가하여 검사를 비활성화할 수 있다. 
> 그러나 이는 type을 any로 두는 것과 같이 타입 안정성을 줄이는 행위이므로 가급적이면 코드를 리팩토링하자.

## 읽기 전용 속성

- readonly로 선언된 속성은 선언된 위치 또는 생성자에서 초깃값만 할당할 수 있다. 해당 속성은 읽을 수만 있고 값을 변경할 수는 없다.
- let이 아닌 const로 변수를 할당하는 것과 유사하다.(추후 초기화 및 새로운 값 할당이 불가능하다.)

> readonly는 TS에만 존재하는 기능이다. 그러므로 진정한 읽기 전용 보호가 필요하다면 private이나 getter를 사용하자.

## 타입으로서의 클래스

- `타입 시스템에서의 클래스는 런타임 시의 값(클래스 객체 자체)과 타입으로서도 생성된다.`
- 클래스의 동일한(이름이 같은) 멤버를 모두 포함하는 모든 객체 타입을 그 클래스에 할당할 수 있는 것으로 간주한다.
  - 이는 타입스크립트의 구조적 타이핑이 선언되는 방식이 아니라 객체의 형태만 고려하기 때문이다.

```ts
class Example {
  test: () => void;

  constructor() {
    this.test = () => {
      console.log('test');
    }
  }
}

const example: Example = { 
  test() {
    console.log('객체입니다.')
  }
}
// class 생성자를 통해 생성된 객체가 아닌, 객체 리터럴임에도 동일한 멤버만 갖고 있다면 할당이 가능하다.
```

> 어떻게 보면 조금은 허술해보일 수 있지만, 실제로 우리가 코드를 작성할 때 class로 선언된 타입을 가져와서 객체 리터럴로 값을 할당하는 일은 거의 없다.

## 클래스와 인터페이스

- 클래스 이름 뒤에 `implements` 키워드와 인터페이스 이름을 추가함으로써 클래스의 인스턴스가 이 인터페이스를 따른다고 선언할 수 있다.

```ts
interface ExampleImpl {
  test: string;
}

class Example implements ExampleImpl {
  test: string;
  constructor() {
    this.test = 'test입니다.';
  }
}
```

- 타입스크립트는 인터페이스에서 클래스의 메서드 또는 속성 타입을 유추하지 않는다.

```ts
interface ExampleImpl {
  test: string;
  do: (something: string) => void;
  play: (something: string) => void;
}

class Example implements ExampleImpl {
  test; // 프로퍼티는 에러 안남.
  play; // 프로퍼티로 선언된 함수 자체는 에러 안남.
  constructor() {
    this.test = 'test입니다.';
    this.play = (something) => { // Parameter 'something' implicitly has an 'any' type. 여기서 타입 에러 발생
      console.log(`Let's play ${something}`)
    }
  }
  do(something) { // Parameter 'something' implicitly has an 'any' type.
    console.log(something);
  }
}

// 즉, something 인자는 string으로 추론되지 못하고 any 타입으로 지정된다. 그러니까 위와 같은 any 타입 오류가 나오는 것이다.
```

> 인터페이스를 구현하는 것은 순전히 안정성 검사를 위함이다! 

## 다중 인터페이스 구현

```ts
interface FirstImpl {}
interface SecondImpl {}

class Example implements FirstImpl, SecondImpl {} // 두개의 인터페이스의 규칙을 모두 다 지켜야 한다.
```

## 추상 클래스(Abstract Class)

일부 메서드의 구현을 선언하지 않고, 하위 클래스가 해당 메서드를 제공할 것을 예상하여 기본 클래스를 만들 때 사용한다.
- class 앞에 abstract를 추가한다.
- 생성자로서의 역할은 하지 못한다.

## 멤버 접근성

- public(default): 어디서나 접근 가능
- protected: 해당 클래스 내부 또는 하위 클래스에서만 접근 가능
- private: 해당 클래스 내부에서만 접근 가능

> JS에서는 암묵적으로 protected는 `_` 표시하고 private는 `#`으로 정말 private의 기능을 가진다.
> TS의 멤버 접근성은 타입 시스템에만 존재하는 반면, JS의 private는 런타임에도 존재한다.

## 정적 필드 제한자

- class에서 static 또한 readonly가 적용된다.