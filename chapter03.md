# 3장 유니언과 리터럴

<img width="703" alt="스크린샷 2023-04-09 오후 12 02 08" src="https://user-images.githubusercontent.com/81420856/230752118-a523c8eb-7bca-4db3-9c83-67301b61dfbd.png">


이번 주에 드디어 로컬에서 타입스크립트를 실행해봤습니다.

# 유니언과 리터럴

### 리터럴 타입

- 해당 타입의 가능한 모든 리터럴 값의 집합

```tsx
let something:"something"

something = "hello" // Error

let bin = "";  // -> 빈 문자열의 Type은 String이다?

bin = ":)" // OK
```

### 엄격한 null 검사

**3.4.2 참 검사를 통한 내로잉**

- “자바스크립트에서 false, 0, -0, 0n, null, undefined, NaN 처럼 falsy로 정의된 값을 제외한 모든 값은 모두 참입니다.”
    
    → 0n이나 null NaN이 falsy인가요?.? 
    

