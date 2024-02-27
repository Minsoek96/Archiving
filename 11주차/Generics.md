# Generics

## Genrics란?

단일 타입이 아닌 다양한 타입에서 작동하는 컴포넌트를 작성할 수 있다.  
사용자는 제니릭을 통해 여러 타입의 컴포넌트나 자신만의 타입을 사용할 수 있다.  

제니릭이 없다면, identity 함수에 특정 타입을 주어야 한다.

```jsx
function identity(arg: number): number {
    return arg;
}
```

### any를 사용하면 되잖아??

함수의 `arg`가 어떤 타입이든 받을 수 있다는 점에서 제니릭잊만, 실제로 함수가 반환할때 어떤 타입인지에 대한 정보는 잃게 된다. 만약 `number`타입을 넘긴다 해도 `any`타입이 반환된다는 정보만 얻을뿐이다.

> 1. 어떤 타입이 반환되는지 알 수 없다.
> 2. `any`를 사용하면 타입스크릡트의 타입 체킄 기능을 무시하게되어, 예상치 못한 오류가 발생할 가능성이 높아진다.  


### 타입변수 사용 

```jsx
function idnetity<Type>(arg: Type): Type {
    retun arg;
}
```

`identity`함수에 `Type`라는 타입 변수를 추가했다. 
Type은 유저가 준 인수의 타입을 캡처하고(예-`number`), 이 정보를 나중에 사용할 수 있게 한다.

identity 함수는 타입을 불문하고 동작하므로 제니릭이라고 할 수 있다.  
any를 쓰는 것과 다르게 인수와 반환타입에 number를 사용한 함수만큼 정확하다.  

### 호출방법 

함수에 타입인수를 전달

```jsx
let output = identity<string>("myString"); // 출력 타입은 'string'입니다.
      
let output: string
```

타입 추론 방법

```jsx
let output = identity("myString"); // 출력 타입은 'string'입니다.
      
//let output: string 추론
```

### 여러 예제

타입스크립트에서 제네릭을 사용하여, 다양한 타입의 배열을 받아 첫 번째 요소를 반환하는 함수 `getFirstElement`를 만드세요. 이 함수는 어떤 타입의 배열이든 처리할 수 있어야 한다.

```jsx
function getFirestElement<T>(arr: T[]):T {
	return arr[0]
}
```

다음 요구사항을 만족하는 제네릭 인터페이스 `Wrapper`를 만들어주세요:

- `Wrapper` 인터페이스는 어떤 타입의 `value`도 저장할 수 있어야 한다.
- `getValue` 메서드는 해당 `value`를 반환해야 한다.

```jsx
interface Wrapper<T> {
	value: T;
	getValue(): T;
}
```

클래스 `ValueWrapper`를 만들어주세요. 이 클래스는 다음 기능을 수행해야 합니다:

- 생성자에서 하나의 매개변수를 받아 `value`속성에 저장합니다.
- `getValue` 메서드는 `value`속성의 값을 반환합니다.