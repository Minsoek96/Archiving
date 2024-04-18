# dehydrate

`React Query`의 `dehydrate`함수는 현재 쿼리 클라이언트 캐시의 "고정된 스냅샷"을 생성한다. 이 스냅샷은 나중에 클라이언트 측에서 캐시를 `hydrate`하는 데 사용할 수 있다. 즉, 서버 측 렌더링(SSR)와 클라이언트 측 렌더링(CSR)간에 데이터를 매끄럽게 전달하는 데 도움이 된다.

## 핵심 기능

- 고정된 캐시 표현 생성 : `dehydrate`함수는 캐시의 내용을 변경할 수 없는 고정된 스냅샷을 만든다.
- `HydrationBoundary` 또는 `hydrate`를 통한 수화

### 사용 사례

- **서버에서 클라이언트로 미리 페치된 쿼리 전달** : `SSR`과정에서 `ReactQuery`를 사용하여 데이터를 미리 페치하고, 캐시 상태를 `dehydrate`할 수 있다. 이 스냅샷은 초기 `HTML`와 함께 클라이언트에게 전송된다. 클라이언트가 이를 받으면 캐시를 `hydrate`하여 추가적인 네트워크 요청 없이 데이터에 즉시 액세스할 수 있다.
- **localStorage에 쿼리 저장** : `dehydrate` 함수를 사용하여 캐시 스냅샷을 만들고 `localStorage`와 같은 지속적인 저장소에 저장할 수도 있다. 이를 통해 후속 페이지 로드 시 캐시 상태를 복원하여 불필요한 데이터 재요청을 방지하여 성능을 향상 시킬수 있다.

---

## **함수 인자**

- `client (필수)`: `dehydrate`하려는 `QueryClient` 인스턴스
- `options (선택적)`: 데이터 스냅샷 생성에 대한 옵션 객체
  - `shouldDehydrateMutation (선택적)`: 캐시에 있는 각 `Mutation` 객체에 대해 호출되는 콜백 함수이다. 이 함수는 뮤테이션을 스냅샷에 포함할지 여부를 `true` 또는 `false`로 반환한다. 기본적으로는 일시 중지된 뮤테이션만 포함된다. 기본 동작을 유지하면서 함수를 확장하고 싶다면 `defaultShouldDehydrateMutation`을 import하여 반환 문에 포함시킬 수 있다.
  - `shouldDehydrateQuery (선택적)`: 캐시에 있는 각 `Query` 객체에 대해 호출되는 콜백 함수이다. 이 함수는 쿼리를 스냅샷에 포함할지 여부를 `true` 또는 `false`로 반환한다. 기본적으로는 성공적인 쿼리만 포함된다. 기본 동작을 유지하면서 함수를 확장하고 싶다면 `defaultShouldDehydrateQuery`을 import하여 반환 문에 포함시킬 수 있다.


## **반환 값**

- `dehydratedState: DehydratedState`: 나중에 `queryClient` 를 수화하는 데 필요한 모든 정보가 포함되어 있는 스냅샷이다. 이 응답 객체의 정확한 형식은 공개 API의 일부가 아니므로 변경될 수 있다. 따라서 이 객체의 구조에 의존해서는 안 된다.

- **직렬화:** 반환된 `dehydratedState` 는 자동으로 직렬화(예: JSON 포맷)되지 않는다. `localStorage`에 저장하거나 네트워크를 통해 전송해야 한다면 직렬화를 직접 수행해야 한다 (예: `JSON.stringify` 사용).

**중요 참고 사항**

- 기본적으로 `dehydrate` 함수는 현재 성공적인 쿼리만 스냅샷에 포함한다. `shouldDehydrateQuery` 옵션을 사용하여 이 동작을 커스터마이징할 수 있다.
- 반환된 스냅샷 (`dehydratedState`) 을 직접 수정해서는 안 된다. 이 스냅샷의 목적은 `hydrate` 함수에 전달하여 캐시를 복원하는 것이다.