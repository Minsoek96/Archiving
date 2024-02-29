# 보충 학습 

## 인덱서블 타입(IndexableTypes)

```jsx
interface StringArray {
    [index: number]: string;
}
```

`[index: number] : string` 해당 부분은 인덱스 시그니처(index signature)라고 불린다.  
`StringArray` 인터페이스를 구현하는 모든 객체가, 숫자 타입의 인덱스로 접근했을 때 `string` 타입의 값을 반환해야함 을 의미한다.

## 클래스 타입(Class Type)

```jsx
interface Animal {
	makeSound(): void
}
```

>클래스가 특정 계약(contract)을 충족시키도록 명시적으로 강제화 하는 방법  
클래스가 인터페이스를 구현하기 위해서는 `implements` 키워드를 사용한다.  
클래스는 인터페이스에 정의된 모든 메서드와 프로퍼티를 구현해야만 한다.  

### Implements

```jsx
class Dog implements Animal {
	makeSound() {
		console.log("Woof! Woof");
	}
}
```

### 다중 인터페이스

```jsx
interface Movable {
    move(): void;
}

interface Controllable {
    control(direction: string): void;
}

class Robot implements Movable, Controllable {
    move() {
        console.log("Robot is moving");
    }

    control(direction: string) {
        console.log(`Robot is moving to ${direction}`);
    }
}

```