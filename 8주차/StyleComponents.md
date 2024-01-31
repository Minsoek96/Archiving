# StyleCompoents 

> React 프로젝트에서 주로 사용되는 CSS-in-JS 라이브러리  
Styled-component는 React컴포넌트 시스템의 스타일링을 위한 CSS를 어떻게 향상시킬수 있을지에 대한 고민의 결과이다.  

## 특징 

- **자동 중요 CSS** :  페이지에 렌더링 컴포넌트를 추적하여 사용자가 필요한 스타일만 로드한다.  
- **클래스 이름 버그 없음** : 스타일에 대해 고유한 클래스 이름을 생성한다. 중복, 겹침, 철자 오류에 대해 걱정할 필요가 없다.
- **유지보수 편리** : 모든 스타일링이 특정 컴포넌트에 연결되어 있기 때문에 관리가 편리하다.  
- **간단한 동적 스타일링** : props나 global theme을 이용하여 간단하고 직관적으로 스타일을 적용할 수 있다.  

## 설치 

```bash
npm i styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

`.swrc 파일 작성`
```json 
{
	"jsc": {
		"experimental": {
			"plugins": [
				[
					"@swc/plugin-styled-components",
					{
						"displayName": true,
						"ssr": true
					}
				]
			]
		}
	}
}
```

## 가이드 

**기본 사용법**  
태그가 지정된 템플릿 리터럴을 사용하여 컴포넌트의 스타일을 지정한다. 

```jsx
import styled from 'styled-components';

const Paragraph = styled.p`
	color: #00F;
`;

export default function Greeting() {
	return (
		<Paragraph>
			Hello, world!
		</Paragraph>
	);
}
```

**스타일 상속**  
다른 컴포넌트의 스타일을 상속하는 새 컴포넌트를 쉽게 만들려면 `styled()`생성자로 감싸기만 하면 된다.  

```jsx
import styled from 'styled-components';

const Paragraph = styled.p`
	color: #00F;
`;

const BigParagraph = styled(Paragraph)`
	font-size: 5rem;
`;

export default function Greeting() {
	return (
		<BigParagraph>
			Hello, world!
		</BigParagraph>
	);
}
```
  
스타일 메서드는 전달된 `className` 프로퍼티를 DOM요소에 첨부하기만 하면 자체 컴포넌트나 타사 컴포넌트 모두에서 완벽하게 작동한다.

```jsx
import styled from 'styled-components';

function HelloWorld({ className }: React.HTMLAttributes<HTMLElement>) {
	return (
		<p className={className}>
			Hello, world!
		</p>
	);
}

const Greeting = styled(HelloWorld)`
	color: #00F;
`;

export default Greeting;
```

**Props에 따라 조정하기**  
스타일 컴포넌트의 템플릿 리터럴에 함수("interpolations")를 전달하여 해당 Prop에 따라 조정할 수 있다.  

```jsx
const Button = styled.button<{ $primary?: boolean; }>`
  /* Adapt the colors based on primary prop */
  background: ${props => props.$primary ? "#BF4F74" : "white"};
  color: ${props => props.$primary ? "white" : "#BF4F74"};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
`;

render(
  <div>
    <Button>Normal</Button>
    <Button $primary>Primary</Button>
  </div>
);
```  

**as 다형성 porps**  
스타일링된 컴포넌트가 렌더링하는 HTML 태그나 React컴포넌트를 동적으로 변경할 수 있다.  

```jsx
const StyledComponent = styled.a`
  /* 여기에 공통 스타일을 정의한다*/
`;

// 'as' prop을 사용하여 'StyledComponent'를 'button'으로 렌더링
<StyledComponent as="button">버튼처럼 보이는 링크</StyledComponent>

// 같은 'StyledComponent'를 'a' 태그로 렌더링
<StyledComponent href="/path">링크</StyledComponent>
```  

>스타일 컴포넌트는 렌더링 메서드 외부에서 정의하는 것이 중요하다. 그렇지 않으면 매 렌더링 마다 새로 생성된다. 렌더 메소드 내에서 스타일 컴포넌트를 선언하면 캐싱에 방해가되고 렌더링 속도를 크게 늦출 수 있다. 