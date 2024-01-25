# WebAPIs-History

## History  

`History` 사용자의 탐색 기록을 다루는 데 사용되는 객체  

### 주요 메서드 

`pushState(stateObj, title, url)`

- 탐색 기록에 새로운 항목을 추가  
- 페이지를 새로 고치지 않고 URL을 변경할 수 있게 해주어 SPA에서 효율적이다.  
- `stateObject`은 사용자가 그 히스토리 항목으로 돌아왔을때 필요한 정보를 담고 있다.
- `history.pushState({ page:1 }, 'title', '/about')`

`replaceState(stateObj, title, url)`  

- pushState는 탐색기록을 남지지 않는다.  
- 사용자가 뒤로 가기 버튼을 클릭하면 대체된 페이지가 아닌 이전 탐색 기록 으로 이동 한다.  

`go(n)`  

- 탐색 기록에서 지정된 위치로 이동한다.  
- go(-1) === back() **이전 페이지로 이동**
- go(1) === forward() **다음 페이지로 이동**

## React Router

### Link

- `Link` 컴포넌트는 다른 경로로 이동할 때 사용된다.  
- `<a>` 태그와 비슷하지만, 페이지를 새로고침 하지 않고 클라이언트 사이드에서 라우팅을 처리한다.  

```jsx
<Link to="/about"> About </Link>
```

### NavLink  

-  `Link`와 유사한 기능 하지만 현재 브라우저 URL와 링크의 목적지가 일치할 때 스타일이나 클래스를 추가할 수 있다.

```jsx
<NavLink to="/home" activeClassName='active'>
    HOME
</NavLink>
```  

### Navigate  

- `Navigate` 컴포넌트는 사용자를 즉시 다른 경로로 리디렉션 한다.  

```jsx
<Navigate to='/login'/>
```

### useNavigate  

- React Router V6부터 도입된 훅  
- useNavigate를 호출하여 Navigate 처럼 사용할 수 있다.  

```jsx 
import { useNavigate } from 'react-router-dom';

function MyComponent() {
    const navigate = useNavigate();

    const goToHomePage = () => {
        navigate('/'); // 홈페이지로 이동
    };

    const goBack = () => {
        navigate(-1); // 이전 페이지로 이동
    };

		const handleSubmit = () => {
        navigate('/dashboard', { replace: true }); 
				// 현재 페이지를 대시보드로 대체
    };

    return (
        <div>
            <button onClick={goToHomePage}>Go to Home</button>
            <button onClick={goBack}>Go Back</button>
        </div>
    );
}
```