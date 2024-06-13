# 게시판 리팩토링

## 리팩토링 이전의 구조 파악

### 라우팅 구조

```bash
📦qna
┣ 📂answer
┃ ┗ 📜[id].tsx
┣ 📜list.tsx
┗ 📜question.tsx
```

### 컴포넌트 구조

```bash
📦QNA
┣ 📂Answer
┃ ┣ 📂FeedBack
┃ ┃ ┣ 📜FeedBackItems.tsx
┃ ┃ ┗ 📜FeedBackView.tsx
┃ ┣ 📜AnswerDragView.tsx
┃ ┣ 📜AnswerWirte.tsx
┃ ┣ 📜BoardItem.tsx
┃ ┣ 📜EditMenuBar.tsx
┃ ┗ 📜QuestionContent.tsx
┣ 📂Question
┃ ┣ 📜EssayDetailItems.tsx
┃ ┣ 📜EssayDetailView.tsx
┃ ┗ 📜QuestionWirte.tsx
┣ 📜BoardListItem.tsx
┣ 📜BoardListView.tsx
┗ 📜PageNation.tsx
```

- Answer : 답변하기 관련
  - FeedBackItems : 댓글 관련 아이템
  - FeddBackView : 댓글 보여주기
- AnswerDragView : 드래그 관련
- AnswerWirte : 답변 작성
- BoardItem : ???
- EditMenuBar : 편집 메뉴

- Question

  - EssayDetaillItems : 수필 상세보기 아이템
  - EssayDetailView : 수필 상세보기
  - QuestionWrite : 질문 작성

- Board
  - BoardListItem : 게시판 아이템
  - BoardListView : 게시판 보여주기
  - PageNation : 페이지 네이션 토글 ?

## 컴포넌트 구조 개선 계획

> 네이밍 전략 : 의도가 분명할 것  
=> 자기소개서에 대한 질문의 답변에 특화된 게시판

### 자기소개서에 대한 질문와 답변에 특화된 게시판

- **Answer : 유지**
  - FeedBackItems : 유지
  - FeedBackView → FeedBackLayout : 변경
- AnswerDragView : 커스텀훅 고려 useTextDragView
- AnswerWirte : 분할 고려
- BoardItem → QuestionDetail 질문 메인 상세 보기
- EditMenuBar : 유지

- **Question**
  - EssayDetailItems → CoverLetterDetailItems : 자기소개서 상세보기 아이템
  - EssayDetailView → CoverLetterDetailLayout : 자기소개서 레이아웃
  - QuestionWrite → QuestionComposer : 질문 작성기

### Board 디렉토리에서 별도 관리

- BoardListItem → BoardListItem : BoardItem
- BoardListView → BoardLayout : 게시판 레이아웃
- PageNation : 커스텀훅으로 관리 고려 usePageNation

## 컴포넌트별 리팩토링 계획

### Board

주요 문제점

- 현재는 CoverLetterQnA에 집중 되어 있습니다.

개선 방안

- 다른 카테고리를 추가해도 활용 할 수 있는 형태로 변형
- 유지보수가 효율적인 구조
- 특정 데이터에 한정 되지 않을 것

리팩토링 적용 구조

```bash
📦Board
 ┣ 📜BoardList.tsx
 ┣ 📜BoardListHead.tsx
 ┣ 📜BoardListItem.tsx
 ┗ 📜BoardListLayout.tsx
```

<!-- markdownlint-disable MD033 -->

<details>
  <summary>1. 리팩토링 적용 코드 보기</summary>
  
  ```jsx
import React, { useEffect } from "react";
import styled from "styled-components";

import BoardList from "./BoardList";
import BoardListHead from "./BoardListHead";

import { RootReducerType } from "../redux/store"
import { useSelector } from "react-redux";

const HEAD_ITEM = ["번호", "제목", "닉네임"];
const ID_KEY = "id";
const ROUTE_TEMPLATE = "/qna/answer/";

const BoardListLayout = () => {
//TODO :: Manager위임 작업시 변경
const { boardInfos } = useSelector(
(state: RootReducerType) => state.board.boardInFoList,
);
return (
<BoardListViewStyle>
<BoardListHead headItem={HEAD_ITEM} />
<BoardList
boardInfos={boardInfos ?? []}
idKey={ID_KEY}
displayKeys={["id", "title", "nickname"]}
route={ROUTE_TEMPLATE}
/>
</BoardListViewStyle>
);
};

export default BoardListLayout

````

</details>

<!-- markdownlint-enable MD033 -->

- 특정 데이터에 제한 받지 않고 원하는 형태의 게시판 생성에 용이한 구조로 개선하였습니다.
- `Props`는 상수화 하여 관리가 쉬워지고 유지보수의 효율이 높아졌습니다.

<!-- markdownlint-disable MD033 -->

<details>
  <summary>2. 리팩토링 적용 코드 보기</summary>

```jsx
interface BoardListProps<T> {
  boardInfos: T[];
  idKey: keyof T;
  displayKeys: (keyof T)[];
  route?: string;
}

const BoardList = <T,>({
  boardInfos,
  idKey,
  displayKeys,
  route,
}: BoardListProps<T>) => {
  return (
    <>
      {boardInfos?.map(info => (
        <BoardListItem
          key={String(info[idKey])}
          item={info}
          idKey={idKey}
          displayKeys={displayKeys}
          route={route}
        />
      ))}
    </>
  );
};

export default BoardList;
````

</details>

<!-- markdownlint-enable MD033 -->

- 제니릭 타입을 사용하여 타입의 안정성을 지키면서 특정 데이터에 한정 되지 않게되었습니다.

### Question: 질문하기

주요 리팩토링 사항

- 컴포넌트 분할
- 절차적인 성향의 코드
- 직관적 네이밍

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 코드 보기</summary>

```jsx
useEffect(() => {
  if (essayTitle !== "") {
    const findEssayId = essayList.filter((list) => list.title === essayTitle)[0]
      .essayId;
    setEssayId(findEssayId);
  }
}, [essayTitle]);
```

---

```jsx
const handleChangeTitle = (title: string) => {
  setEssayTitle(title);
  const findEssay = essayList.find((list) => list.title === title);
  if (findEssay) setEssayId(findEssay?.essayId);
};
```

</details>

<!-- markdownlint-enable MD033 -->

- setEssayTitle을 onChange Props으로 전달하여 EssayTitle의 상태를 변형하고 그 변화에 따라 useEffect를 동작 시키는 방식이 불필요한 절차적인 과정이라고 생각이 되었습니다.

- handleChangeTitle을 onChange props로 전달하여 직접적으로 변경하는 방식으로 변환 하였습니다.

### Answer : 답변하기 (게시글상세)

주요 문제점 :

- 리덕스, 모달 등 너무 많은 상태를 직접적으로 관여
- 의미를 알 수 없는 매직넘버
- 너무나도 많은 관심사

```jsx
useEffect(() => {
  if (!isNaN(Number(boardId.id))) {
    dispatch(getBoardDetail(Number(boardId.id)));
    setRemoveID(Number(boardId.id));
  }
}, [router.query]);
```

- 페이지가 있음에도 컴포넌트에서 router.query 를 다루고 있음
- 페이지내에서 관리하는것으로 변경

```jsx
correctionText.length < 30 || correction.targetAnswer === "";
{
  correction.targetQuestion !== 0 ? correction.targetQuestion : "";
}
list === feedBackIndex ? "active_button" : " ";
```

- 의미 파악이 불명확한 매직넘버

AnswerDragView 같은 경우 현재 드래그에 관련된 상태도 직접적으로 관리하고 선택된 QNA에 대한 렌더링도 담당합니다. 단일 책임의 원칙에 따라 VIEW를 분리하고, 드래그의 상태와, 선택된 QNA의 상태를 따로 관리 할 예정입니다.

### AnswerDragView

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 이전 코드</summary>

```jsx
import React, { useEffect, useState } from "react";
import styled from "styled-components";
import { useSelector } from "react-redux";
import { RootReducerType } from "../../redux/store";

import { CorrectionItem } from "./AnswerWirte";
import { ScrollBar } from "@/styles/GlobalStyle";
import useModal from "@/hooks/useModal";

const EssayDragStyle = styled.div`
  ${ScrollBar}
  background-color: #191919;
  color: #dad6d1;
  padding: 15px;
  height: 100vh;
  width: 100%;
  word-break: break-all;
  overflow-x: hidden;
  .essay_title {
    text-align: center;
  }
`;

const EssayDragItems = styled.div`
  margin: 30px auto;
  font-family: "Noto Sans KR", sans-serif;
  .essay_question {
    margin-bottom: 15px;
    > span {
    }
  }
  .essay_answer {
    font-weight: normal;
    font-size: 15px;
    line-height: 1.7em;
  }
`;

const EssayQuestionContainer = styled.div`
  border-top: 1.5px solid #e4dddd;
  border-bottom: 1.5px solid #e4dddd;
  padding: 12px;
  margin: 15px auto;
  font-size: 14px;
  line-height: 1.45;
`;

const EssayAnswerContainer = styled.div`
  padding: 12px;
  line-height: 1.6;
`;

interface EssayDragProps {
  onChange: React.Dispatch<React.SetStateAction<CorrectionItem>>;
}

/**드래그 첨삭 기능을 가진 자소서 View */
const EssayDragView = ({ onChange }: EssayDragProps) => {
  const [selectedData, setSelectedData] = useState({
    dragTitleNumber: 0,
    qnaId: 0,
    selectedText: "",
    start: 0,
    end: 0,
    added: false,
  });
  const boardList = useSelector(
    (state: RootReducerType) => state.board.boardList
  );

  const { setModal, Modal } = useModal();
  useEffect(() => {
    handleApply();
  }, [selectedData]);

  /**드래그 첨삭 탐색*/
  const handleSelect = (dragTitleNumber: number, qnaId: number) => {
    const selectedText = window.getSelection()?.toString();
    if (selectedText !== "" && selectedData.added) {
      setModal({
        contents: {
          title: "Message",
          content: "이미 선택한 문장이 존재합니다.",
        },
      });
      return;
    }

    if (selectedText && selectedText !== "") {
      const range = window.getSelection()?.getRangeAt(0);
      const start = range?.startOffset || 0;
      const end = range?.endOffset || 0;

      setSelectedData({
        dragTitleNumber,
        qnaId,
        selectedText,
        start,
        end,
        added: true,
      });
    }
  };

  /**드래그 첨삭 적용 */
  const handleApply = () => {
    const { dragTitleNumber, selectedText, qnaId } = selectedData;
    onChange({
      targetQuestion: dragTitleNumber,
      targetAnswer: selectedText,
      targetQuestionIndex: qnaId,
    });
  };

  /**현재 드래그 첨삭 삭제 */
  const handleRemove = () => {
    setSelectedData({
      dragTitleNumber: 0,
      qnaId: 0,
      selectedText: "",
      start: 0,
      end: 0,
      added: false,
    });
  };

  return (
    <EssayDragStyle>
      <h2 className="essay_title">
        {boardList[0]?.essayTitle && boardList[0].essayTitle}
      </h2>
      {boardList[0]?.qnaList &&
        boardList[0].qnaList.map((list, idx) => (
          <EssayDragItems key={list.qnaId}>
            <EssayQuestionContainer>
              <h4 className="essay_question">
                <span>질문:</span> {list.question}
              </h4>
            </EssayQuestionContainer>
            <EssayAnswerContainer>
              <p
                className="essay_answer"
                onMouseUp={() => handleSelect(idx + 1, list.qnaId)}
              >
                답변:
                {selectedData.added && selectedData.qnaId === list.qnaId ? (
                  <>
                    {list.answer.substring(0, selectedData.start)}
                    <span
                      style={{ backgroundColor: "#e69a0def" }}
                      onClick={handleRemove}
                    >
                      {selectedData.selectedText}
                    </span>
                    {list.answer.substring(
                      selectedData.end,
                      list.answer.length
                    )}
                  </>
                ) : (
                  list.answer
                )}
              </p>
            </EssayAnswerContainer>
          </EssayDragItems>
        ))}
      <Modal />
    </EssayDragStyle>
  );
};

export default EssayDragView;
```

</details>

<!-- markdownlint-enable MD033 -->

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 이후 코드</summary>

```jsx
/**드래그 첨삭 기능을 가진 자소서 View */
const AnswerDragView = () => {
  const { handleCorrection, selectedText, removeCorrection, Modal } =
    useDragCorrection();
  const { dispatchChangeCorrection } = userQnaManager();
  const { boardList } = useQnaManager();

  useEffect(() => {
    handleApply();
  }, [selectedText]);

  /**드래그 첨삭 적용 */
  const handleApply = () => {
    dispatchChangeCorrection({
      targetQuestion: selectedText.dragTitleId,
      targetAnswer: selectedText.selectedText,
      targetQuestionIndex: selectedText.targetId,
    });
  };

  return (
    <AnswerDragStyle>
      <h2 className="essay_title">{boardList?.essayTitle}</h2>
      {boardList?.qnaList &&
        boardList.qnaList.map((list, idx) => (
          <AnswerDragItem
            key={list.qnaId}
            onSelect={handleCorrection}
            onRemove={removeCorrection}
            index={idx}
            selectedText={selectedText}
            list={list}
          />
        ))}
      <Modal />
    </AnswerDragStyle>
  );
};

export default AnswerDragView;

-----

const AnswerDragItem = ({
  list,
  onSelect,
  onRemove,
  selectedText,
  index,
}: AnswerDragItemProps) => {
  const [selectedColor, setSelectedColor] = useState("#e69a0def");
  const { selectedText: preSelectedText } = selectedText;
  const { qnaId, question, answer } = list;

  const isSelected = selectedText.added && selectedText.targetId === list.qnaId;
  const 시작점부터타겟까지문장 = list.answer.substring(0, selectedText.start);
  const 타겟부터끝나는지점문장 = list.answer.substring(
    selectedText.end,
    answer.length,
  );

  const DraggedAnswer = () => {
    return (
      <>
        {시작점부터타겟까지문장}
        <span style={{ backgroundColor: selectedColor }} onClick={onRemove}>
          {preSelectedText}
        </span>
        {타겟부터끝나는지점문장}
      </>
    );
  };

  return (
    <EssayDragItems>
      <EssayQuestionContainer>
        <h4 className="essay_question">
          <span>질문:</span> {question}
        </h4>
      </EssayQuestionContainer>
      <EssayAnswerContainer>
        <span>답변</span>
        <p
          className="essay_answer"
          onMouseUp={() => onSelect(index + 1, qnaId)}
        >
          {isSelected ? <DraggedAnswer /> : answer}
        </p>
      </EssayAnswerContainer>
      <ColorPicker
        onSelectColor={color => {
          setSelectedColor(color);
        }}
      />
    </EssayDragItems>
  );
};
```

</details>

<!-- markdownlint-enable MD033 -->

[`개선 사항`]

컴포넌트 분리 : `AnswerDragItem` 드래그 대상 아이템을 렌더링

커스텀 훅 사용 :

- `useDragCorrection`: 드래그 상태를 관리
- `userQnaManger`: 유저의 상태와 관련된 Qna 디스패치를 관리
- `useQnaManger`: 서버의 상태와 관련된 Qna 디스패치를 관리

하드 코딩 제거 : `boardList[0]?essayTitle`와 같이 직접적인 인덱스 참조가 있었는데 리팩토링 이후에는 이러한 하드 코딩이 제거되었습니다.

매직 넘버 제거 : `list.answer.substring(0, selectedText.start)` 와 같은 쉽게 이해 할수 없는 문법을 알기쉽게 `{시작점부터타겟까지문장}`직관적인 표현을 해주었습니다.

### AnswerWirte

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 이전 코드</summary>

```jsx
export type CorrectionItem = {
  targetQuestion: number,
  targetAnswer: string,
  targetQuestionIndex: number,
};

const AnswerWirte = () => {
  const router = useRouter();
  const [correction, setCorrection] =
    useState <
    CorrectionItem >
    {
      targetQuestion: 0,
      targetAnswer: "",
      targetQuestionIndex: 0,
    };
  const [correctionText, setCorrectionText] = useState < string > "";
  const [feedBackIndex, setFeedBackIndex] = useState < number > 0;
  const [removeID, setRemoveID] = useState < number > 0;
  const [isFeedBackClear, setIsFeedBackClear] = useState < boolean > false;
  const [isViolation, setIsViolation] = useState < boolean > false;
  const [isOpenModal, setIsOpenModal] = useState < boolean > false;
  const dispatch = useDispatch();
  const boardId = router.query;

  const boardList = useSelector(
    (state: RootReducerType) => state.board.boardList
  );

  const boardQnAIdList = useSelector(
    (state: RootReducerType) => state.board.qnaIdList
  );

  useEffect(() => {
    if (!isNaN(Number(boardId.id))) {
      dispatch(getBoardDetail(Number(boardId.id)));
      setRemoveID(Number(boardId.id));
    }
  }, [router.query]);

  const handleSubmit = () => {
    if (correctionText.length < 30 || correction.targetAnswer === "") {
      setIsOpenModal(true);
      setIsViolation(true);
      setTimeout(() => {
        setIsViolation(false);
      }, 10);
      return;
    }
    const data = {
      qnaId: correction.targetQuestionIndex,
      feedbackTarget: correction.targetAnswer,
      feedbackContent: correctionText,
    };
    dispatch(writeFeedback(data));
    setCorrection({
      targetAnswer: "",
      targetQuestion: 0,
      targetQuestionIndex: 0,
    });
    handleClear();
  };

  const handleClear = () => {
    setIsFeedBackClear(true);
    setTimeout(() => {
      setIsFeedBackClear(false);
    }, 10);
  };

  const handleFeedBackIndex = (qnaId: number) => {
    setFeedBackIndex(qnaId);
  };

  const handleChangeFeedBack = (feedBackText: string) => {
    setCorrectionText(feedBackText);
  };

  const handleCloseModal = () => {
    setIsOpenModal(false);
  };
  return (
    <AnswerWirteStyle>
      <Card size={{ width: "80%", height: "45vh", flex: "row" }}>
        <SwitchContainer>
          <LeftContainer>
            {boardList &&
              boardList.map((list, i) => (
                <BoardItem key={list.boardId} {...list} />
              ))}
          </LeftContainer>
          <RigthContainer>
            <AnswerDragView onChange={setCorrection} />
          </RigthContainer>
        </SwitchContainer>
        {boardList.length > 0 && boardList[0].owner && (
          <EditMenuBar boardID={removeID} />
        )}
      </Card>

      <Card size={{ width: "80%", height: "35vh", flex: "col" }}>
        <CorrectionContainer>
          <span className="correction_title">
            현재 선택된 문장:{" "}
            {correction.targetQuestion !== 0 ? correction.targetQuestion : ""}
          </span>
          <h4 className="correction_sentence">{correction.targetAnswer}</h4>
        </CorrectionContainer>

        <CommentTop>
          <TextArea
            handleChangeText={handleChangeFeedBack}
            violation={isViolation}
            clear={isFeedBackClear}
          ></TextArea>
        </CommentTop>

        <CommentFooter>
          <ControlLeftButtons>
            {boardQnAIdList.map((list, i) => (
              <CustomButton
                className={list === feedBackIndex ? "active_button" : " "}
                Size={{ width: "40px", font: "15px" }}
                text={`${i + 1}`}
                onClick={() => handleFeedBackIndex(list)}
                key={list}
              ></CustomButton>
            ))}
          </ControlLeftButtons>

          <ControlRightButtons>
            <CustomButton
              text="비우기"
              onClick={handleClear}
              Size={{ width: "150px", font: "15px" }}
            ></CustomButton>
            <CustomButton
              text="작성"
              onClick={handleSubmit}
              Size={{ width: "150px", font: "15px" }}
            ></CustomButton>
          </ControlRightButtons>
        </CommentFooter>
      </Card>

      <FeedBackView targetNumber={feedBackIndex}></FeedBackView>
      {isOpenModal && (
        <Modal
          isOpen={isOpenModal}
          onClose={handleCloseModal}
          contents={{
            title: "경고",
            content: `Target질문을 선택해주세요.
               피드백은 30자이상 작성하세요. `,
          }}
        ></Modal>
      )}
    </AnswerWirteStyle>
  );
};

export default AnswerWirte;
```

</details>

<!-- markdownlint-enable MD033 -->

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링 이후 코드</summary>

```jsx
  export type CorrectionItem = {
    targetQuestion: number;
    targetAnswer: string;
    targetQuestionIndex: number;
  };

  const AnswerLayout = () => {
    //TODO:: 기본적인 컴포넌트 분리완료, 컴포넌트 모듈화 하고 상태에 대한 로직 분리하기, props에 따라 리덕스 고려하기 !!!!!!!
    //Corection에 대한 상태 떄문에 props 드릴링이 발생한다. 리덕스로 변경
    return (
      <AnswerWirteStyle>
        <AnswerDetailView />
        <FeedBackComposer />
        <TargetFeedBackView />
      </AnswerWirteStyle>
    );
  };

  export default AnswerLayout;

  -----

  const AnswerDetailView = () => {
    const { boardList, isUpdated } = useQnaManager();
    if (isUpdated) return <p>유저의 질문을 받아오는중입니다.</p>;
    return (
      <Card size={{ width: "80%", height: "45vh", flex: "row" }}>
        <SwitchContainer>
          <LeftContainer>
            <BoardItem {...boardList} />
          </LeftContainer>
          <RigthContainer>
            <AnswerDragView />
          </RigthContainer>
        </SwitchContainer>
        {boardList.owner && <EditMenuBar boardID={boardList.boardId} />}
      </Card>
    );
  };

  -------

  const FeedBackComposer = () => {
    const { dispatchChangeFeed, targetFeed } = userQnaManager();
    const { qnaIdList } = useQnaManager();
    const {
      selectedCorrection,
      handleChangeFeedBack,
      isViolation,
      isFeedBackClear,
      handleSubmit,
      handleClear,
      Modal,
    } = useFeedBackLogic();

    //TODO:: 기본적인 컴포넌트 분리완료, 컴포넌트 모듈화 하고 상태에 대한 로직 분리하기, props에 따라 리덕스 고려하기 !!!!!!!

    //Corection에 대한 상태 떄문에 props 드릴링이 발생한다. 리덕스로 변경
    return (
      <Card size={{ width: "80%", height: "35vh", flex: "col" }}>
        <CorrectionView {...selectedCorrection} />
        <CommentTop>
          <TextArea
            handleChangeText={handleChangeFeedBack}
            violation={isViolation}
            clear={isFeedBackClear}
          ></TextArea>
        </CommentTop>
        <FeedBackFooter
          handleFeedBackIndex={dispatchChangeFeed}
          handleSubmit={handleSubmit}
          handleClear={handleClear}
          qnaIdList={qnaIdList}
          feedBackIndex={targetFeed}
        />
        <Modal />
      </Card>
    );
  };

  export default FeedBackComposer;

  -----

  const TargetFeedBackView = () => {
    const {
      feedbackList,
      isUpdated,
      dispatchGetFeedList,
      dispatchUpdateFeedBack,
    } = useFeedManager();
    const { targetFeed } = userQnaManager();

    useEffect(() => {
      if (targetFeed !== 0) {
        dispatchGetFeedList(targetFeed);
      }
    }, [targetFeed, isUpdated]);

    return (
      <FeedBackViewStyle>
        {feedbackList?.map(feedback => (
          <TargetFeedBackItems
            key={feedback.feedbackId}
            handleUpdateFeedBack={dispatchUpdateFeedBack}
            {...feedback}
          ></TargetFeedBackItems>
        ))}
      </FeedBackViewStyle>
    );
  };

  export default TargetFeedBackView;
```

</details>

<!-- markdownlint-enable MD033 -->

[`개선 사항`] :

네이밍 개선 : AnswerWrite → AnswerLayout

컴포넌트 분리 : 각각의 관심사별로 책임을 분리 하였습니다.

- AnswerDetailView : 유저의 질문을 세부사항을 관리 합니다.
- FeedBackComposer : 피드백을 작성 관련 기능을 관리 합니다.
- TargetFeedBackView : 유저가 선택한 피드백을 렌더링 합니다.

리팩토링 이전 디렉토리 구조

```bash
📦Answer
 ┣ 📂FeedBack
 ┃ ┣ 📜FeedBackItems.tsx
 ┃ ┣ 📜FeedBackView.tsx
 ┣ 📜AnswerDragView.tsx
 ┣ 📜AnswerWrite.tsx
 ┣ 📜BoardItem.tsx
 ┗ 📜EditMenuBar.tsx
```

리팩토링 디렉토리 구조

```bash
📦Answer
 ┣ 📂AnswerDetail
 ┃ ┣ 📜AnswerDetailView.tsx
 ┃ ┣ 📜AnswerDragItem.tsx
 ┃ ┣ 📜AnswerDragView.tsx
 ┃ ┣ 📜BoardItem.tsx
 ┃ ┣ 📜ColorPicker.tsx
 ┃ ┗ 📜EditMenuBar.tsx
 ┣ 📂FeedBack
 ┃ ┣ 📜CorrectionView.tsx
 ┃ ┣ 📜FeedBackComposer.tsx
 ┃ ┗ 📜FeedBackFooter.tsx
 ┣ 📂TargetFeedBack
 ┃ ┣ 📜TargetFeedBackItems.tsx
 ┃ ┗ 📜TargetFeedBackView.tsx
 ┗ 📜AnswerLayout.tsx
```

- 코드의 구조와 가독성이 크게 개선되었습니다. 컴포넌트의 네이밍과 디렉토리 구조의 변화를 통해 구조를 이해하기 쉬워졌습니다.
- 특정 기능 또는 컴포넌트를 찾기도 용이 해졌습니다.
