# EDIT? : 자기소개서 작성 & 편집

해당 컴포넌트는 유저의 자소서 리스트를 관리하기 위한 목적으로 작성되었습니다.

- EDIT라는 명칭은 의미를 파악하기 힘들었습니다.
- 이전 문제와 동일하게 컴포넌트의 관심사 분리 & 매직넘버 문제가 있었습니다.
- Props드릴링 문제도 발생했습니다.

## 상세 문제점 파악

- 표현이 애매한 도메인명
  - EDIT : 편집 무엇을 편집?
- 복잡한 페이지 라우팅 구조 & 명칭
  - myEssay : 자소서 확인 페이지
  - edit : 자소서 작성 페이지 라우팅
  - edit essay=2 : 자소서 수정 페이지
- idx로 이루어진 map 키값

## EDIT 해결 방안

- 대표 도메인명 수정
  - coverLetter: 자기소개서
- 복잡한 라우팅 & 명칭 개선
  - coverLetter : 자소서 확인 페이지
  - coverLetter/new : 자소서 작성 페이지
  - coverLetter/[id]/edit: 자소서 편집 페이지
- 컴포넌트 관심사 분류
- UUID4()를 키값으로 사용하기
- 트리가 깊은 경우 리덕스 활용

## 자기소개서 리팩토링 적용전

### 유저 자소서 리스트 관리

- 유저가 선택한 자소서 넘버 상태를 관리 하기 위한 로직이 Props드릴링 구조의 주요 원인이라 판단되었습니다.

### 유저 자소서 생성 & 편집

> 생성과 편집의 스타일이나 로직이 비슷하다는 이유로 하나의 컴포넌트에서 이를 처리하려 했던 안일한 접근이 문제였습니다.

- `isEdit` 상태에 대한 의존도가 높아져 가독성이 떨어지고 유지보수 측면에서 비효율적이었습니다.
- 삼항 연산자를 이용하여 기능을 분류하는 방식은 절차적인 성향의 코드에 가까워 컴포넌트 간의 의존도를 높이고 유지보수 측면에서 매우 비효율적이며 가독성을 떨어트렸습니다.

---

[ `해결방안`]

- **컴포넌트 분리**: 생성 기능과 편집 기능을 별도의 컴포넌트로 분리하였습니다.
  커스텀 훅 활용: 비슷하고 겹치는 비즈니스 로직은 커스텀 훅으로 분리하여 상태 관리를 효율적으로 하였습니다.
- **명칭 변경**: `handle ~~` 형태의 불명확한 명칭을 최대한 직관적으로 변경하였습니다.
- **고유 키 값 사용**: 서버에서 id 값을 지정해주기 때문에 key 값에 크게 신경 쓰지 않았으나, CRUD 관련 컴포넌트의 맵핑에서는 key 값을 유니크한 값으로 지정하는 것이 옳다고 판단하여 `uuid4()`를 채택하였습니다.

### EDIT 리팩토링 적용전 코드

<!-- markdownlint-disable MD033 -->

<details>
  <summary>리팩토링전 코드 </summary>

```jsx
type QnAEditorProps = {
  isEdit: boolean;
};

const QnAEditor = ({ isEdit }: QnAEditorProps) => {
  {
    /**리덕스에 대한 선언 */
  }
  const router = useRouter();
  const dispatch = useDispatch();
  /**나의 자소서를 호출중 판단여부 */
  const readEssayLoading = useSelector(
    (state: RootReducerType) => state.essay.loading,
  );
  /**나의 자소서 리스트 */
  const readEssayList = useSelector(
    (state: RootReducerType) => state.essay.readEssayList,
  );
  /**관리자의 샘플 리스트 */
  const templateReducer: InitiaState = useSelector(
    (state: RootReducerType) => state.template,
  );

  const [qnaLists, setQnALists] = useState<QnAList[]>([]);
  const [templateTitle, setTemplateTitle] = useState<string>("커스텀자소서");
  const [mainTitle, setMainTitle] = useState<string>("");
  const [qnaContent, setQnAContent] = useState<qnaList>([]);
  const [isOpenModal, setIsOpenModal] = useState<boolean>(false);
  const [modalMsg, setModalMsg] = useState<string>("");
  const essayId = isEdit
    ? (JSON.parse(router.query.essayId as string) as number)
    : 0;

  /** ESSAY의 로딩이 완료가되면 useState에 반영한다. */
  useEffect(() => {
    if (!readEssayLoading) {
      if (readEssayList[0].title === "") {
        return;
      }
      setQnALists(readEssayList);
      setTemplateTitle(readEssayList[0]?.title);
    }
  }, [readEssayLoading]);

  /** 템플릿의 로딩이 완료가되면 useState에 반영한다. */
  useEffect(() => {
    if (!templateReducer.loading) {
      const customTemplate = {
        templateId: 10000,
        title: "커스텀자소서",
        qnaList: [],
      };
      setQnALists(cur => [customTemplate, ...templateReducer.templateList]);
    }
  }, [templateReducer.templateList]);
  /** 수정모드에서는 기존의 리스트를 반환, 작성모드에서는 선택된 템플릿 리스트를 반환 */
  const getQuestionItem = useCallback(() => {
    if (isEdit) {
      return qnaLists;
    }
    const filteItem = qnaLists.filter(list => list.title === templateTitle);
    return filteItem;
  }, [templateTitle, qnaLists]);

  const handleChangeMainTitle = (title: string) => {
    setMainTitle(title);
  };

  /**질문FORM 추가 함수 */
  const handleAddQnA = () => {
    if (templateTitle === "") {
      return;
    }
    setQnALists(prevLists => {
      const newLists = [...prevLists];
      const filterIndex = newLists.findIndex(a => a.title === templateTitle);
      const newContent = [...newLists[filterIndex].qnaList, " "];
      newLists[filterIndex].qnaList = newContent;
      return newLists;
    });
  };

  /**현재 질문리스트 수정 */
  const handleChangeQnA = (index: number, question: string, answer: string) => {
    const newList = [...qnaContent];
    newList[index].question = question;
    newList[index].answer = answer;
    setQnAContent(newList);
  };

  /**현재 질문리스트의 정보를 전달 */
  const handleQnASelect = (index: number, question: string, answer: string) => {
    const filterList = qnaContent[index];
    if (filterList) {
      return;
    } else {
      setQnAContent(curList => [
        ...curList,
        { qnaId: index, question, answer: answer },
      ]);
    }
  };

  /**필터링후 자소서 작성 반영 */
  const handleSubmit = () => {
    const filterJudge = qnaContent.filter(
      a => a.answer === "" || a.question === "",
    ).length;
    if (!isEdit && mainTitle === "") {
      setIsOpenModal(true);
      setModalMsg("메인 제목을 입력해주세요");
      return;
    }
    if (qnaContent.length < 1) {
      setIsOpenModal(true);
      setModalMsg("질문과 답변은 1개이상 작성해주세요.");
      return;
    }
    if (filterJudge > 0) {
      setIsOpenModal(true);
      setModalMsg("질문 답변의 내용을 채워주세요");
      return;
    }
    const data = {
      title: isEdit
        ? mainTitle === ""
          ? templateTitle
          : mainTitle
        : mainTitle,
      qnaList: qnaContent,
    };
    if (isEdit) {
      dispatch(updateEssay(data, essayId));
      router.replace("/myEssay");
      return;
    }
    dispatch(addEssay(data, Number(Cookies.get("useId"))));
    router.replace("/myEssay");
  };


  /**모드1은 액션이 없는 모달, 모드2 삭제액션이 담긴 모달 */
  const handleModal = (mode: number) => {
    if (mode === 1) {
      setIsOpenModal(false);
    }
    if (mode === 2) {
      setModalMsg(`${essayId}번 자소서를 정말 삭제하시겠습니까?`);
      setIsOpenModal(true);
    }
  };

  /**자소서 삭제*/
  const handleDeleteEssay = () => {
    dispatch(deleteEssayList(essayId));
    router.push("/myEssay");
  };

  return (
    <QnAEditorStyle>
      <h2>{isEdit ? "자소서 수정하기" : "자소서 작성하기"}</h2>

      <TopContainer>
        <QnAListTitle onChange={handleChangeMainTitle} />
        <ControlMenu
          Size={{ width: `${v.lgItemWidth}`, height: "40px" }}
          value={templateTitle}
          optionList={qnaLists}
          onChange={setTemplateTitle}
        />
        {qnaLists &&
          getQuestionItem().map(list => (
            <QuestionItmesContainer
              key={isEdit ? list.essayId : list.templateId}
            >
              {list.qnaList &&
                list.qnaList.map((qna, idx) => (
                  <QuestionItem
                    key={idx}
                    content={qna}
                    onChange={handleChangeQnA}
                    curInfo={handleQnASelect}
                    index={idx}
                  ></QuestionItem>
                ))}
            </QuestionItmesContainer>
          ))}
      </TopContainer>

      <BottomContainer>
        <BiPlus onClick={handleAddQnA}></BiPlus>
        <ControlButtonContainer>
          <div>
            <CustomButton
              Size={{ width: "150px", font: "20px" }}
              onClick={() => router.push("/myEssay")}
              text="뒤로가기"
            />
          </div>
          <div>
            {isEdit ? (
              <CustomButton
                Size={{ width: "150px", font: "20px" }}
                onClick={() => handleModal(2)}
                text="삭제하기"
              />
            ) : (
              <></>
            )}
            <CustomButton
              Size={{ width: "150px", font: "20px" }}
              onClick={handleSubmit}
              text={isEdit ? "수정완료" : "작성완료"}
            />
          </div>
        </ControlButtonContainer>
      </BottomContainer>

      {isOpenModal && (
        <Modal
          isOpen={isOpenModal}
          onClose={() => handleModal(1)}
          onAction={isEdit ? handleDeleteEssay : () => handleModal(1)}
          contents={{
            title: "경고",
            content: modalMsg,
          }}
        ></Modal>
      )}
    </QnAEditorStyle>
  );
};

export default QnAEditor;
```

</details>

<!-- markdownlint-enable MD033 -->

- 주석이 없으면 이해할수 없는 코드와 구조

### 리팩토링 적용후(코드 일부)

```jsx
const CoverLetterEdit = () => {
  const [coverLetterTitle, setCoverLetterTitle] = useState < string > "";
  const router = useRouter();
  const moveCoverLetterMainPage = "/coverLetter";
  const { id } = router.query;

  const { qnaList, setQnAList, deleteQnAList, changeQnAList, addQnAList } =
    useCoverLetterCreatorLogic();

  const {
    loading,
    targetQnAData,
    getDetailEssayList,
    coverLetterMainTitle,
    changeCoverLetter,
    deleteCoverLetter,
  } = useCoverLetterManager();

  useEffect(() => {
    getDetailEssayList(Number(id));
  }, []);

  useEffect(() => {
    if (!loading) {
      setQnAList(targetQnAData);
      setCoverLetterTitle(coverLetterMainTitle);
    }
  }, [targetQnAData]);

  if (loading) return <p>로딩중...</p>;

  return (
    <CoverLetterCreatorContainer>
      <MainTitle>자소서 수정하기</MainTitle>
      <CoverLetterTitle
        value={coverLetterTitle}
        onChange={(e) => setCoverLetterTitle(e.target.value)}
        placeholder="자소서 제목"
      ></CoverLetterTitle>
      {qnaList.map((qna, i) => (
        <CoverLetterQuestionItems
          key={qna.qnaId}
          item={qna}
          onDelete={deleteQnAList}
          onUpdate={changeQnAList}
        ></CoverLetterQuestionItems>
      ))}
      <BiPlusStyled onClick={addQnAList}></BiPlusStyled>
      <ControllerBtns>
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() => router.push(moveCoverLetterMainPage)}
          text={"뒤로가기"}
        />
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() => deleteCoverLetter(Number(id))}
          text={"삭제하기"}
        />
        <CustomButton
          Size={{ width: "150px", font: "20px" }}
          onClick={() =>
            changeCoverLetter(Number(id), coverLetterTitle, qnaList)
          }
          text={"수정완료"}
        />
      </ControllerBtns>
    </CoverLetterCreatorContainer>
  );
};

export default CoverLetterEdit;
```

- 비즈니스 로직을 외부로 분리하여 가독성과 관리가 용이 해졌습니다.
- 비즈니스 로직을 커스텀훅으로 분리하여 재 사용할 수 있는 가능성이 생겼습니다.
- 유지보수 측면에서 효율적으로 변했습니다.
- 컴포넌트 자체는 UI렌더링과 간단한 상태관리에 집중되어 단일 책임 원칙을 지켰습니다.

## 리팩토링 후 구조

### 컴포넌트

```bash
📦CoverLetter
┣ 📂edit
┃ ┗ 📜CoverLetterEdit.tsx
┣ 📂hooks
┃ ┣ 📜useControlTemplate.ts
┃ ┣ 📜useCoverLetterCreatorLogic.ts
┃ ┗ 📜useCoverLetterManager.ts
┣ 📂new
┃ ┣ 📜CoverLetterCreator.tsx
┃ ┗ 📜CoverLetterQuestionItems.tsx
┣ 📜CoverLetter.tsx
┣ 📜CoverLetterItems.tsx
┣ 📜CoverLetterList.tsx
┗ 📜CoverLetterPreView.tsx
```

### 라우팅

```bash
📦coverLetter
 ┣ 📂new
 ┃ ┗ 📜index.tsx
 ┣ 📂[id]
 ┃ ┗ 📂edit
 ┃ ┃ ┗ 📜index.tsx
 ┗ 📜index.tsx
```

### 리덕스

```bash
📦Essay
 ┣ 📂server
 ┃ ┣ 📜actions.tsx
 ┃ ┣ 📜reducer.tsx
 ┃ ┗ 📜types.tsx
 ┗ 📂user
 ┃ ┣ 📜actions.ts
 ┃ ┣ 📜reducer.ts
 ┃ ┗ 📜types.ts
```
