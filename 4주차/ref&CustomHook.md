# Hook의 규칙

## 최상위(at the Top Level)에서만 Hook을 호출해야 한다.

: 반복문, 조건문, 혹은 중첩된 함수 내에서 Hook을 호출하지 말자.  
: 함수형 컴포넌트나 Custom Hook 내부에서만 Hook을 호출해야 한다.

## Custom Hook

React에서 자신만의 훅을 만드는 훅

- 로직을 재사용하기 위한 제일 쉬운 방법
- 평범하게 Extra Function을 수행하면 된다.
- Hook은 “use”로 시작하는 camelCase로 이름을 붙이면 된다.

```jsx
function useFetchProducts() {
    const [products, setProducts] = useState<Product[]>([]);

    useEffect(() => {
        const fetchProducts = async () => {
            const url = 'http://localhost:3000/products';
            const response = await fetch(url);
            const data = await response.json();
            setProducts(data.products);
        };

        fetchProducts();
    }, []);

    return products;
}
```
