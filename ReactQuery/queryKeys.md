# QueryKeys 

> ReactQuery에서 `key`는 캐싱 및 데이터 식별 메커니즘의 핵심 요소 

## Query key의 역할 

- 각 쿼리를 식별하는 데 사용된다.
- React Query는 `key`를 사용하여 내부 캐시에 데이터를 저장 하고
- 해당 데이터에 대한 요청을 만들때 마다 `key`를 사용하여 캐시된 데이터를 조회한다.
  
---

## Key의 구조 

> 쿼리 키가 직렬화 가능하고  
쿼리의 데이터에 고유한 경우

v4 이전에는 `queryKey`를 `string`, `array`의 형태로 사용할 수 있었다.  
v4 이후 `queryKey`는 최상위 배열이어야 하며, 단일 문자로 이루어진 배열에서부터 여러 문자열과 중첩 된 객체로 이루어진 복잡한 배열에 이르기까지 다양할 수 있다.
  
---

## Simple Query Key 

> 가장 단순한 형태는 문자열로 이루어진 배열이다. 

- 일반적인 목록/ 인덱스 리소스
- 비계층적인 리소스 

```jsx
// 할 일 목록
useQuery({ queryKey: ['todos'], ... })

// 뭔가 다른 것, 뭐든 좋아!
useQuery({ queryKey: ['something', 'special'], ... })
```
  
---

### 비계층적인 리소스 ??

> 비계층적 구조는 모든 항목이 동일한 레벨에 위치하며, 특정 항목이 다른 항목을 포함하지 않는 단순한 리스트 형태로 구성된다.

```json
{
  "employees": [
    {"id": 1, "name": "Alice", "position": "Developer"},
    {"id": 2, "name": "Bob", "position": "Designer"},
    {"id": 3, "name": "Charlie", "position": "Project Manager"}
  ]
}
```

`employees`는 직원 목록을 나타내며, 각 직원은 동일한 레벨의 객체로 존재한다.  
`Alice,Bob,Charlie`는 모두 같은 `employess` 리스트에도 포함되어 있지만,부모 - 자식 관계가 없다. 각 직원 정보는 서로 독립적이다.

### 계층적인 리소스 ??

> 계층적인 리소스는 서로 다른 요소들이 상하 관계나 부모- 자식 관계를 형성하는 데이터 구조를 의미한다.

예를들어, 온라인 쇼핑몰에서의 제품정보를 생각해 볼 수 있다.

1. 최상위 계층은 제품의 카테고리 (전자제품, 의류, 가전)
2. 각 카테고리 내에는 하위 카테고리가 존재할 수 있다.
3. 하위 카테고리내에는 구체적인 '제품'들이 위치한다. 
4. 각 제품에 대한 상세정보나, '리뷰', '평점'등의 추가 정보가 붙을수 있다.
  
---

### Array Keys With Varialbles

쿼리가 데이터를 고유하게 설명하기 위해서 더 많은 정보가 필요한 경우  
문자열과 어떤 수의 직렬화 가능한 객체를 포함한 배열을 사용할 수 있다.  

- 계층적 또는 중첩된 리소스 
  - 특정 항목을 고유하게 식별하기 위해 ID, 인덱스, 또는 다른 원시값을 전달하는 것이 일반적이다. 
- 추가 매개변수가 있는 쿼리  
  - 추가 옵션의 객체를 전달하는 것이 일반적  

```jsx
// 개별 할 일
useQuery({ queryKey: ['todo', 5], ... })

// "미리보기" 형식의 개별 할 일
useQuery({ queryKey: ['todo', 5, { preview: true }], ...})

// "완료된" 할 일 목록
useQuery({ queryKey: ['todos', { type: 'done' }], ... })
```

---

### 쿼리 키는 결정론적으로 해시된다.

객체의 키 순서와 관계없이 다음의 모든 쿼리가 동일하게 간주된다는 것을 의미한다.

```jsx
useQuery({ queryKey: ['todos', { status, page }], ... })
useQuery({ queryKey: ['todos', { page, status }], ...})
useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })
```

그러나 다음 쿼리 키들은 동일 하지 않다. 배열 항목의 순서가 중요하다 !!! 

```jsx
useQuery({ queryKey: ['todos', status, page], ... })
useQuery({ queryKey: ['todos', page, status], ...})
useQuery({ queryKey: ['todos', undefined, page, status], ...})
```
  
---

### 쿼리 함수가 변수에 의존하는 경우,  변수를 쿼리 키에 포함 시키자

쿼리 키는 가져오는 데이터를 고유하게 설명하기 때문에, 변경되는 쿼리 함수에서 사용하는 변수를 포함해야 한다.

```jsx
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ['todos', todoId],
    queryFn: () => fetchTodoById(todoId),
  })
}
```

쿼리 키는 쿼리 함수의 종속 변수 역할을 하므로, 종속 변수를 쿼리 키에 추가하면 쿼리가 독립적으로 캐시되고 변수가 변경될 때마다 자동으로 쿼리가 새로 고침된다.

