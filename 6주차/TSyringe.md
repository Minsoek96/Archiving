# TSyringe

## TSyringe?

---
> Tsyringe는 자바스크립트 및 타입스크립트를 위한 가벼운 의존성 주입(Dependency injection, DI) 라이브러리이다.

외부에서 의존성을 주입받아 모듈간의 결합도를 낮추고 유지보수성을 향상시키는 방식  
React에서는 PropsDrilling문제를 해결할 수 있는 방법 중 하나 

### 주요기능  


**@데코레이터**:  
클래스나 메소드, 프로퍼티, 메타데이터를 추가하여, 해당 클래스가 의존성 주입을 받을 수 있도록 명시해준다.

- `@injectable() :`  
-- 클래스가 의존성 주입을 받을 수 있음을 나타낸다.  
-- 해당 데코레이터가 없으면 TSyringe 컨테이너는 클래스의 인스턴스를 생성할 수 없다.

- `@singleton() :`  
-- 클래스를 싱글턴으로 등록한다.  
-- 해당 클래스의 인스턴스는 전역에서 오직 하나만 생성되고 공유된다.  

- `@inject() :`  
-- 생성자의 매개변수에 사용되며, 특정 타입의 의존성을 주입하도록 명시한다.  
  
**Container:**  
TSyringe의 `container` 객체는 의존성 주입을 관리한다.  
의존성이 주입된 클래스의 인스턴스를 생성하고 관리할 수 있다.  

- `resolve<T>() :`  
-- 특정 클래스의 인스턴스를 생성하고, 필요한 모든 의존성을 주입한다.  
-- EX: `const myClassInstance = container.resolve(MyClass)`  

- `register() :`  
-- 컨테이너에 클래스, 값, 팩토리 등을 수동으로 등록한다.  
-- EX: `container.register("MyService", { useClass: MyService })`  

- `registerSingleton() :`  
-- 특정 클래스를 싱글턴으로 컨테이너에 등록한다.  
-- EX: `container.registerSingleton(MyService)`  
  
## 의존성 주입(Dependency injection)  

---
> 객체의 생성과 사용의 관심을 분리하는 설계  

의존성 주입은 클래스나 모듈이 직접적으로 자신이 의존하는 객체를 생성하지 않고, 외부(주로 IoC 컨테이너)로 부터 필요한 의존성을 받는 설계  

의존성 ?  
서비스로 사용할 수 있는 객체  
클래스 A가 클래스 B를 사용하기 때문에 A는 B에 의존적  

### 예시  

클래스 A가 클래스 B에 의존하는 경우,  
클래스 A는 클래스 B의 인스턴스를 직접 생성하지 않는다.  
대신 IoC컨테이너 같은 외부 시스템이 필요한 시점에 클래스 B의 인스턴스를 클래스 A에 제공한다.  

### 장점  

의존성이 중앙화 관리되기 때문에 코드의 재사용성이 높아지고, 유지보수가 용이해진다.  
의존성을 외부에서 주입받기 때문에 테스트에 유리하다.  

## 제어의 역전(Inversion of Control, IoC)  

---
제어의 역전은 프로그램의 흐름을 사용자가 직접 제어하는 대신 외부 시스템에 위임하는 원칙이다.  

### IoC 컨테이너  

의존성을 관리하고, 필요한 곳에 의존성을 주입하는 역할을 한다.  
모든 의존성을 등록하고 관리하며, 요청에 따라 해당 의존성을 제공한다.  

IoC(제어의역전) 컨테이너라는 매개체를 두고 필요한 모든 모듈들을 등록해둔다.  
사용처에서 직접 생성하지 않고 필요시 IoC컨테이너가 의존성을 주입하는 방식  
해당 과정에서 의존하는 모듈의 생성과 해제와 같은 모든 제어 과정을 프레임워크에게 주면서 제어의 역전이 발생한다.  

### reflect-metadata  

javaScript의 데코레이터와 메타데이터 관리를 위한 라이브러리  
리플렉션 기능을 활용하여 런타임에 객체의 메타데이터를 읽거나 수정할 수 있게 해준다.  

**Reflection API** : 객체, 클래스, 메소드에 대한 메타데이터를 정의하고, 런타임 시 조회할 수 있게 해준다.  

**메타데이터** : 데이터에 대한 데이터로, 동작 방식에 대한 정보를 제공한다.  
-> 클래스, 함수, 프로퍼티 등에 관련된 정보를 담고 있는 데이터  

**데코레이터** : TS최신 문법에 속하는 데코레이터는 클래스, 메소드, 속성 등에 추가적인 동작을 부여할 수 있는 선언적인 패턴이다.  

TS에서 클래스 데코레이터를 사용하여 클래스에 메타데이터를 추가하고, 이 메타데이터를 나중에 읽어서 특정 동작을 수행하는 것이 가능하다.  

```jsx
import "reflect-metadata";

class Example {
	@Reflect.metadata('customMetadataKey", "customMetadataValue")
	method() {}
}

console.log(Reflect.getMetadata("customMetadataKey", new Example(), "method"));
```

Reflect.metadata 데코레이터를 사용하여 메소드에 메타데이터를 추가하고, Reflect.getMetadata를 통해 해당 메타데이터를 조회한다.

## singleton(싱글톤)

---
> 싱글톤 패턴은 특정 클래스의 인스턴스를 1개만 생성되는 것을 보장하는 디자인 패턴이다.

### 핵심개념 

싱글톤 클래스는 전역적으로 단 하나의 인스턴스만 존재한다.  
어디서든 해당 인스턴스에 접근할 수 있어 전역적인 접근점을 제공한다.  
싱글톤은 자신의 생성과 생명주기를 스스로 관리한다.  

### 장점  

인스턴스를 생성할 때 마다 메모리가 낭비되는데 반복적으로 생성하지 않아 메모리를 절약할 수 있다. 또한 공유 자원에 대한 접근을 관리할 수 있다.  

### 단점  

글로벌 상태가 코드의 결합도를 높이고 단위 테스트를 어렵게 만든다. 

```jsx 
var Singleton = (function () {
	var instance;

	function createInstance() {
		var object = new Object("I am the instance");
		return object;
	}
	
	return {
		 getInstance: function() {
				if (!instance) {
						instance = createInstance();
				}
				return instance;
		}
};
}) ();

var instance1 = Singleton.getInstance();
var instance2 = Singleton.getInstance();
console.log(instance1 === instance2);  // true

```