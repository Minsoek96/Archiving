# 인터뷰 샘플 리스트

## 리팩토링 이전 문제점 진단

### 디렉토리 구조

```bash
📦Admin
┣ 📂hooks
┣ 📂Template
┣ 📜AddQuestionItem.tsx
┣ 📜AddQuestionListView.tsx
┗ 📜AddTextInput.tsx
```

- AMDIN 페이지 내에 존재하여 ADMIN 컴포넌트에 위치시켰습니다.
- 초기에 작성한 본인조차 의도를 파악하지 못하였습니다.

### 전체적인 코드 문제

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 이전 코드 보기</summary>

```tsx
const InterViewListView = () => {
  const [selectType, setSelectType] = useState<QuestionType | string>("ALL");
  const [allCheck, setAllCheck] = useState<boolean>(false);
  const [checkList, setCheckList] = useState<number[]>([]);
  const [addInterViewList, setAddInterViewList] = useState<string[]>([]);
  const [isOpenModal, setIsOpenModal] = useState<boolean>(false);
  const dispatch = useDispatch();
  const interViewList = useSelector(
    (state: RootReducerType) => state.interViewQuestion.list
  );
  const interViewListUpdated = useSelector(
    (state: RootReducerType) => state.interViewQuestion.isUpdated
  );
  const authRole = useSelector((state: RootReducerType) => state.auth.role);

  /**컨트롤 메뉴를 변경시 적용 */
  useEffect(() => {
    dispatch(getInterViewQnaList(selectType));
  }, [selectType]);

  /**인터뷰질문이 업데이트 되면 업데이트 적용 */
  useEffect(() => {
    if (interViewListUpdated) {
      dispatch(getInterViewQnaList(selectType));
    }
  }, [interViewListUpdated]);

  /**전체체크 제어 */
  const handleAllCheck = () => {
    setAllCheck(!allCheck);
  };

  /**전체체크 변경된값이 true이면 현재리스트 배열에서 id값만 추출하여 리스트 생성 전체체크 해제시 리스트 초기화*/
  useEffect(() => {
    if (allCheck) {
      const idList = interViewList.map((a) => a.id);
      setCheckList(idList);
      return;
    } else {
      setCheckList([]);
      return;
    }
  }, [allCheck]);

  /**체크여부를 판단후 아이템을 제거하고 추가한다.*/
  const handleAddCheckList = (id: number, isCheck: boolean) => {
    if (isCheck) {
      const removeItem = checkList.filter((a) => a !== id);
      setCheckList(removeItem);
      return;
    } else {
      setCheckList((cur) => [...cur, id]);
      return;
    }
  };

  /**인터뷰질문리스트 삭제 */
  const handleRemoveQuestions = () => {
    const data = {
      ids: checkList,
    };
    setAllCheck(false);
    dispatch(handleDeleteInterViewQnaList(data));
  };

  /**인터뷰 영상촬영을 위한 질문리스트 추가 */
  const hadleSetInterViewList = () => {
    const filterItem = interViewList.filter((a, i) => a.id === checkList[i]);
    const questionList = filterItem.map((a, i) => a.questions);
    setAddInterViewList(questionList);
  };

  return (
    <InterViewListViewStyle>
      <Explanation>
        <h2>자신만의 면접 질문 리스트를 만들어주세요.</h2>
        <p>(선택사항)샘플 리스트를 선택하여 추가하면 됩니다.</p>
        <p>(선택사항)자신이 원하는 질문도 추가하면 됩니다.</p>
        <p>
          랜덤셋팅도 있으니 넘어가셔도 됩니다. 자신만의 질문과
          랜덤셋팅을조합할수도있습니다.
        </p>
      </Explanation>
      <SwitchContainer>
        <LeftContainer>
          <Card size={{ height: "450px", width: "100%", flex: "Col" }}>
            <ControlMenu
              value={selectType}
              optionList={InterViewSelectData}
              onChange={setSelectType}
              Size={{ width: "100%", height: "30px" }}
            ></ControlMenu>
            <Container>
              {interViewList &&
                interViewList.map((a, i) => (
                  <InterViewListItem
                    key={a.id}
                    allCheck={allCheck}
                    onChange={handleAddCheckList}
                    {...a}
                  ></InterViewListItem>
                ))}
            </Container>
            <div>
              <CustomButton
                onClick={handleAllCheck}
                text={allCheck ? "전체해제" : "전체선택"}
                Size={{ width: "100px", font: "15px" }}
              />
              {authRole === "ADMIN" ? (
                <CustomButton
                  onClick={handleRemoveQuestions}
                  text={"삭제하기"}
                  Size={{ width: "100px", font: "15px" }}
                />
              ) : (
                <CustomButton
                  onClick={hadleSetInterViewList}
                  text={"항목추가"}
                  Size={{ width: "100px", font: "15px" }}
                />
              )}
            </div>
          </Card>
        </LeftContainer>
        <AddQuestionListView
          qType={selectType}
          addList={addInterViewList}
        ></AddQuestionListView>
      </SwitchContainer>
    </InterViewListViewStyle>
  );
};

export default InterViewListView;
```

</details>

<!-- markdownlint-enable MD033 -->

- 하나의 컴포넌트가 너무 많은 관심사를 가지고 있다.
- 컴포넌트의 명칭이 의미를 파악하기 힘들다.
- 컴포넌트의 추상 부재
- 절차적인 코드
- 매직넘버

### 네이밍 문제

- 관리자가 제공하는 카테고리별 질문 보여주기
  - interViewListView : 전체 레이아웃
  - interViewListItem
- 선택 질문 하단 플레이리스트에 추가
  - ControlMenu

[`하단 컴포넌트`]

- 상단에서 선택한 목록 + 유저가 추가한 목록 보여주기
  - AddQuestionList
  - AddQuestionItem
- 유저가 원하는 질문 추가
  - AddTextInput

## 리팩토링 전략

[`네이밍 변경`]

- 레이아웃 : ExpectedQuestionLayout
- 관리자가 채택한 예상 질문 : ExpectedQuestionSelector
  - 예상 질문 리스트 : ExpectedQuestionList
  - 예상 질문 : ExpectedQuestionItem
  - 예상 질문 카테고리 : QuestionSelector
- 유저가 조합한 예상 질문 리스트 : UserQuestionPlayList
  - 유저가 조합한 질문 : UserQuestionPlayListItem
  - 유저가 질문 추가 : QuestionAdder
  - 액션 버튼 : ActionBtns

[`관심사 분리`]

- 직접적으로 관리하던 CheckList 관련 상태를 커스텀훅으로 분리
- 유저가 선택한 InterviewQuestion 상태를 관련 리덕스 추가
- 유저가 선택한 예상 질문 관련 상태 커스텀훅으로 관리
- ExpectedQuestionList를 분리
- ExpectedQuestionLayOut을 추가하여 전체 구성 관리
- ActionBtns를 추가하여 버튼을 관리
- QuestionSelector 추가하여 유저가 선택하는 질문 카테고리를 관련
- 한눈에 살펴봐도 코드의 가독성은 떨어지고 유지보수의 효율도 좋지 못한 코드의 형태였습니다.

## 리팩토링 적용 후 코드

### Layout

```jsx
import React from "react";
import styled from "styled-components";
import { ColBox, FlexBox, ScrollBar } from "@/styles/GlobalStyle";
import UserQuestionPlayList from "./PlayList/UserQuestionPlayList";
import ExpectedQuestionSelector from "./ExpectedQuestion/ExpectedQuestionSelector";

const ExpectedQuestionLayout = () => {
  return (
    <InterViewListViewStyle>
      <ExplanationContent />
      <SwitchContainer>
        <LeftContainer>
          <ExpectedQuestionSelector />
        </LeftContainer>
        <UserQuestionPlayList />
      </SwitchContainer>
    </InterViewListViewStyle>
  );
};
```

- 기존 로직에 있던 렌더링 부분을 관심사에 별로 분류하여 컴포넌트를 분리 하였습니다.
- ExplanationContent : 설명을 담당
- ExpectedQuestionSelector : 유저가 선택하는 예상 질문 관련 담당
- UserQuestionPlayList : 유저가 인터뷰 면접시 조합하여 만드는 질문 담당

### 유저가 선택한 예상 질문

```tsx
const ExpectedQuestionSelector = () => {
  const { selectedType, dispatchSelectedType, dispatchSelectedQuestions } =
    useEUserQuestionManager();
  const { interViewQuestionList, dispatchRemoveQuestions } =
    useExpectedQuestionManager();
  const { checkList, handleAllCheck, handleCheckList, isAllCheck } =
    useCheckList(interViewQuestionList);

  return (
    <Card size={{ height: "450px", width: "100%", flex: "Col" }}>
      <QuestionSelector
        selectedType={selectedType}
        onChange={dispatchSelectedType}
      />
      <ExpectedQuestionList
        questions={interViewQuestionList}
        isAllCheck={isAllCheck}
        handleCheckList={handleCheckList}
      ></ExpectedQuestionList>
      <ActionBtns
        checkList={checkList}
        onAdd={dispatchSelectedQuestions}
        selectedType={selectedType}
        isAllChecked={isAllCheck}
        onRemove={dispatchRemoveQuestions}
        onToggleAll={handleAllCheck}
        questions={interViewQuestionList}
      ></ActionBtns>
    </Card>
  );
};

export default ExpectedQuestionSelector;
```

- 커스텀훅을 사용하여 비즈니스 로직을 관심 별로 정리 해줬습니다.
- useEUserQuestionManager() : 서버로 부터 제공받는 질문 리덕스 관리
- useExpectedQuestionManager() : 유저가 설정하는 질문 리덕스 관리
- useCheckList : 체크리스트에 대한 관리 위임 커스텀훅

### 유저가 조합한 예상 질문리스트

```tsx
const UserQuestionPlayList = () => {
  const { userQuestion, handleRemoveText, handleAddText, roleAction, Modal } =
    useExpetedPlayListLogic();
  return (
    <AddQuestionListViewStyle>
      <Card size={{ height: "450px", width: "100%", flex: "Col" }}>
        <Container>
          {userQuestion &&
            userQuestion.map((question, idx) => (
              <UserQuestionPlayListItems
                key={idx}
                item={question}
                index={idx}
                handleRemoveText={handleRemoveText}
              ></UserQuestionPlayListItems>
            ))}
        </Container>
        <QuestionAdder
          handleAddQuestion={handleAddText}
          handleCancelQuestion={roleAction}
        />
      </Card>
      <Modal />
    </AddQuestionListViewStyle>
  );
};
```

- 비즈니스 로직을 커스텀훅으로 분리 하여 코드의 가독성을 높였습니다.
- 뷰 라는 관심사의 컴포넌트 책임만을 전담

## 리팩토링 적용후 디렉토리 구조

```bash
📦InterViewQuestion
 ┣ 📂ExpectedQuestion
 ┃ ┣ 📜ActionBtns.tsx
 ┃ ┣ 📜ExpectedQuestionList.tsx
 ┃ ┣ 📜ExpectedQuestionListItem.tsx
 ┃ ┣ 📜ExpectedQuestionSelector.tsx
 ┃ ┗ 📜QuestionSelector.tsx
 ┣ 📂hooks
 ┃ ┣ 📜useEUserQuestionManager.ts
 ┃ ┣ 📜useExpectedPlayListLogic.ts
 ┃ ┗ 📜useExpectedQuestionManager.ts
 ┣ 📂PlayList
 ┃ ┣ 📜QuestionAdder.tsx
 ┃ ┣ 📜UserQuestionPlayList.tsx
 ┃ ┗ 📜UserQuestionPlayListItems.tsx
 ┗ 📜ExpectedQuestionLayout.tsx
```
