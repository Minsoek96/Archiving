# 템플릿

## 템플릿 문제점 분석

템플릿 관련 코드를 리팩토링하기 위해 현재 코드 상태를 평가해 보았습니다.

- 컴포넌트의 명칭이 직관적이지 못했습니다.
- 컴포넌트를 분리하는 기준이 명확하지 않았습니다.
- 전체적으로 주석을 봐도 알아보기 힘든 구조였습니다.
- 리덕스를 사용하고 있음에도 Props 드릴링 문제가 있었습니다.

## 리팩토링 적용 사항

### 서버 상태와 클라이언트 상태를 분리하여 관리

- **이전**: template/redux
- **이후**: template/server - user 디렉토리 구조로 분리

### 템플릿 리스트 컴포넌트의 복잡함 해결

- **해결방안**: 리덕스의 디스패치를 커스텀 훅으로 분리하여 라이브러리에 대한 의존성 문제를 해소하고, 컴포넌트는 단순히 데이터를 가져오고 필요한 인자를 전달하는 역할에 충실하게 하였습니다.
- `useTemplateManager` (서버 관련 상태 디스패치 전담)
- `useUserTemplateManager` (유저 관련 상태 디스패치 전담)

### 복잡한 추상화의 컴포넌트

- **기존 구조**: Template/TemplateList - TemplateQuestionAdd
- **변경된 구조**: Template/TemplateList - TemplateTitleItem - TemplateQuestionAdd - TemplateDetail
- `TemplateList`: 템플릿 리스트를 가져오며, 유저의 행동에 따라 렌더링을 결정합니다.
- `TemplateTitleItem`: 템플릿 아이템을 렌더링하며, 유저 관리 상태를 전달합니다.
- `TemplateQuestionAdd`: 템플릿 추가 기능을 담당합니다.
- `TemplateDetail`: 유저가 선택한 템플릿을 렌더링하며, 선택한 템플릿 삭제 기능을 관리합니다.

### 매직 넘버 제거

- `selectedTemplateList.questions.length < 1` 와 같은 의도 파악이 어려운 조건문을 `isTemplateSelected` 와 같은 변수를 만들어 처리합니다.

## 템플릿 리팩토링 적용후 :

```bash
📦Template
 ┣ 📂server
 ┃ ┣ 📜actions.tsx
 ┃ ┣ 📜reducer.tsx
 ┃ ┗ 📜types.tsx
 ┗ 📂user
 ┃ ┣ 📜actions.ts
 ┃ ┣ 📜reducer.ts
 ┃ ┗ 📜types.ts
```

```jsx
const TemplateList = () => {
  const { templateList, loading, error, getTemplateList } =
    useTemplateManager();
  const { isAddTemplate, setIsAddTemplate } = useUserTemplateManager();

  useEffect(() => {
    getTemplateList();
  }, []);

  if (error) return <p>Error발생</p>;

  return (
    <TemplateStlyed>
      <Card>
        <TemplateTtileList>
          {templateList.map((item) => (
            <TemplateTitleItem key={item.templateId} list={item} />
          ))}
          {!isAddTemplate && (
            <BiPlus onClick={() => setIsAddTemplate(true)}></BiPlus>
          )}
        </TemplateTtileList>
        <TemplateViewController>
          {loading && <p>로딩중</p>}
          {isAddTemplate ? <AddTemplate /> : <TemplateDetail />}
        </TemplateViewController>
      </Card>
    </TemplateStlyed>
  );
};
```

- 최대한 코드 내에서 절차적인 성향을 지우기 위해 커스텀훅을 활용하여 로직을 분리하였습니다.
- 리덕스에 대한 컴포넌트의 의존도를 최소화 하기 위해 리덕스에 대한 내용을 커스텀훅에 감췄습니다.
- `useTemplateManger()` 서버 상태 관련 dispatch 관리
- `useUserTemplateManager()` 클라이언트 상태 관련 dispatch 관리
