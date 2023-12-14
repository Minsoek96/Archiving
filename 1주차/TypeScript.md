### REPL

사용자가 코드를 입력하면 그 코드를 즉시 평가하고 출력하는 대화형 프로그래밍 환경]

- **Read**: 코드를 입력받는다.  
- **Eval** : 코드를 평가 (실행)한다.  
- **Print**: 평가된 결과를 사용자에게 출려가한다.(결과, 오류메시지등)  
- **Loop**: 위의 과정을 반복한다.  
EX: JavaScript의 Node.js 콘솔, 브라우저 개발도구

REPL에서 TypeScript 사용하기

```jsx
npx ts-node
```

### interface vs Type

**문법의 차이**  
interface는 객체의 형태를 취하고 type은 데이터의 형태를 가진다.

```tsx
interface Animal {
  name: string;
}

type Animal = {
  name: string;
};
```

**타입확장**  
interface는 상속의 개념을 가지고 type은 & 교집합 형태로 확장한다.

```tsx
interface Bear extends Animal {
  honey: boolean;
}

type Bear = Animal & {
  honey: Boolean;
};
```

**병합**  
interface는 병합이 가능하지만 type은 병합이 되지않는다.

```tsx
interface Mammal {
  genus: string;
}

interface Mammal {
  breed?: string;
}

interface Mammal {
  genus: string;
  breed?: string;
}
//--------------
type Reptile = {
  genus: stirng;
};

// You cannot add new variables in the same way
type Reptile = {
  breed?: string;
};
```

오류 :  
interface의 이름은 항상 있는 그대로 오류 메시지에 나타난다.

### 타입 추론  

타입스크립가 코드를 해석하는 과정  
TypeScript는 JavaScript의 문법을 알고 있기 때문에 모든 타입을 명시적으로 선언하지 않아도 자동으로 타입을 추론하는 기능이다.

변수 : `let example = “Hello Wolrd!”` ⇒ `example:string`  
함수 :
`getNumber()` ⇒ `getNumber():number`  
`name = “Guest”` ⇒ `name : string = “Guset”`

```tsx
function getNumber() {
  return 123;
}

function getName(name = "Guest") {
  console.log("Hello," + name);
}
```

객체 : `{ name: "User", age: 26}` ⇒ `{name: string, age: number}`

**Best Common Type**  
최선의 방식의 추론  
`let x = [0, 1, null]`=> `x:(number|null)[]`

**Contextual Typing**  
문맥상으로 타입을 결정하는 방식

```tsx
const click = (e) => {
  e; // any
};

document.addEventListener("click", (e) => {
  e; //MouseEvent
});
```

### Union Type vs Intersection Type  

**Union Type**  
여러개의 타입 중 하나일 수 있음을 선언하는 방법

```tsx
type MyBool = true | false;
```

`boolean` 타입을 `true` 또는 `false`로 설명 할 수 있다.

**Intersection Type**  
두 개 이상의 타입을 결합 하여 새로운 타입을 만드는 방법

```tsx
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;
```

### Optional Parameter

? 필수 속성이 아닌 선택적 속성임을 명시

---

https://www.samsungsds.com/kr/insights/typescript.html

https://joshua1988.github.io/ts/why-ts.html
