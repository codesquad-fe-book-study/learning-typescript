# 4장 객체

**4.2 구조적 타이핑**

```tsx
type WithFirstName = {
  firstName: string;
}

type WithLastName = {
  lastName: string;
}

const hasBoth = {
  firstName: "Lucille",
  lastName: "Clifton",
}

**let WithFirstName: WithFirstName = hasBoth;
let WithLastName: WithLastName = hasBoth;
```

이 예제가 이해가 되지 않습니다….

변수 `WithFirstName` 는 type `WithFirstName` 으로 선언됐는데, firstName과 lastName 속성을 가진 `hasBoth` 객체를 할당했는데 왜 Error가 뜨지 않는 것일까요?

**4.2.2 초과 속성 검사**

- 초과 속성 검사는 객체 타입으로 `선언된 위치`에서 생성되는 객체 리터럴에 대해서만 일어납니다. `기존 객체 리터럴` 을 제공하면 초과 속성 검사를 우회합니다.
    
    → “타입스크립트에서 초과 속성을 금지하면 코드를 깨끗하게 유지할 수 있고, 예상한 대로 작동하도록 만들 수 있다.”
    

**4.2.3 중첩된 객체 타입**

“이처럼 중첩된 객체 타입을 고유한 타입 이름으로 바꿔서 사용하면 코드와 오류 메세지가 더 읽기 쉬워집니다.”

→ js에서 항상 고생하는 기능 혹은 목적별 함수 분리 하는 것과 유사한 것 같네요. 물론 목적은 다르지만!

**4.4.1 교차 타입의 위험성 - never**

- 두개의 원시 타입을 교차 타입을 이용해서 동시에 선언하면 never 타입이 된다.
    
    → 왜 Error가 되지 않고, 굳이 never 타입을 만들었을까요?
    

> TypeScript에서 **`never`** 타입은 "절대 발생하지 않을 값"을 나타내는 타입입니다. 즉, 해당 타입의 변수는 어떠한 값도 할당할 수 없습니다.
> 
> 
> **`never`** 타입은 주로 다음과 같은 상황에서 유용합니다:
> 
> 1. 함수가 항상 예외를 던지는 경우
> 2. 함수가 항상 무한 루프에 빠지는 경우
> 3. 타입 가드에서 never를 사용하여 컴파일러에게 이 코드 블록이 끝날 때까지 절대로 실행되지 않는다는 것을 알릴 수 있습니다.
> 
> 이러한 상황에서 **`never`** 타입은 타입 검사를 강화하고, 잠재적인 버그를 미리 방지할 수 있습니다. 또한 **`never`** 타입은 타입 시스템에서의 completeness를 강제하며, 모든 타입 분기가 처리되었는지 확인하는데 유용합니다.
> 
> 따라서 **`never`** 타입은 TypeScript의 강력한 타입 시스템의 일부분으로써, 일부 특정 상황에서는 매우 유용하게 사용될 수 있습니다.