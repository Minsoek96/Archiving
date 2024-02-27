# UtillityTypes

기존 타입을 변환하여 새로운 타입을 생성하는데 사용되는 일종의 제니릭 타입이다.  
유틸리티 타입은 타입스크리븥에 내장되어 있으며, 여러 가지 유용한 타입 변환을 위한 도구를 제공한다.

## 유틸리티 타입 요약

`Partial<T>` : 타입 `T` 의 모든 속성을 선택적으로 만든다.

`Readonly<T>`: 타입 `T`의 모든 속성을 읽기 전용으로 만든다.

`Record<K,T>` : 키 타입 `K`와 값 타입 `T`로 구성된 객체 타입을 생성한다.

`Pick<T, K>` : 타입 `T`에서 속성 `K` 만을 선택하여 타입을 구성한다.

`Exclude<T, U>`: 타입 `T`에서 `U`에 할당 가능한 모든 속성을 제외합니다.

`ReturnType<T>` : 함수 타입 `T`의 반환 타입을 추출합니다.

---

### Partial`<Type>`

`<Type>`집합의 모든 프로퍼티를 선택적으로 타입을 생성한다.

```jsx
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

---

### Readonly`<Type>`

`Type` 집합의 모든 프로퍼티 읽기 전용(readonly)으로 설정한 타입을 생성한다.   
즉 생성된 타입의 프로퍼티는 재할당될 수 없다.

```jsx
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
//Cannot assign to 'title' because it is a read-only property.
```

---

### Record<Keys, Type>

타입 `Type`의 프로퍼티 `키`의 집합으로 타입을 생성한다.

이 유틸리티는 타입의 프로퍼티를 다른 타입에 매핑 시키는데 사용된다.

```jsx
interface PageInfo {
  title: string;
}

type Page = "home" | "about" | "contact";

const nav: Record<Page, PageInfo> = {
  about: { title: "about" },
  contact: { title: "contact" },
  home: { title: "home" },
};

nav.about;
//const nav: Record<Page, PageInfo>
```

---

### Pick<Type, Keys>

`<Type>`에서 프로퍼티 `Keys`의 집합을 선택해 타입을 생성한다.

```jsx
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

todo;
//const todo: TodoPreview
```

---

### Omit<Type, Keys>

`Type` 에서 모든 프로퍼티를 선택하고 키를 제거한 타입을 생성한다.

```jsx
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

todo;
// const todo: TodoPreview
```

---

### Exclude<Type, ExcludedUnion>

`ExcludedUnion` 에 할당할 수 있는 모든 유니온 멤버를 `Type`에서 제외하여 타입을 생성한다.

```jsx
type T0 = Exclude<"a" | "b" | "c", "a">;
//type T0 = "b" | "c"

type T1 = Exclude<"a" | "b" | "c", "a" | "b">;
//type T1 = "c"

type T2 = Exclude<string | number | (() => void), Function>;
//type T2 = string | number
```
