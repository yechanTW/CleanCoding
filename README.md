# CleanCoding

CleanCoding 스터디 입니다. 기존 PR들을 보면서 이전 개선점들 , 또 앞으로 개선해야 할 부분들을 공부합니다.

### [[Web] 클린코드와 리팩토링](https://twinny.atlassian.net/wiki/spaces/TRSITEAM/pages/4510843317/Web)
### [CleanCoding kor](https://github.com/qkraudghgh/clean-code-javascript-ko#%EB%B3%80%EC%88%98variables)

## 1. 변수
[RPD-1861](https://github.com/twinnylab/taras-web/pull/62)

```js
const ButtonRow = ({ onClick, disabled }) => {  ...
-> const ButtonRow = ({ onUpdateUserInfoButtonClick, disabled }) => {  ...

const handleLogin = async () => {   ...
-> const handleLoginFormSubmit = async () => { ...
```

[RPD-1876](https://github.com/twinnylab/taras-web/pull/64)

```js
const openToggle = () => ...
-> const handleToggle = () => ...

onClick={openToggle} ...
-> onClick={handleToggle} ...
// 
```

[RPD-1865](https://github.com/twinnylab/taras-web/pull/65/files)

```js
return { onChangeUserRole };
-> return { handleChangeUserRole };

return { onExpelFromWorkspace };
-> return { handleExpelFromWorkspace };
```

[RPD-1863](https://github.com/twinnylab/taras-web/pull/63)

```js
const ButtonRow = ({ onClick, disabled }) => { ...
-> const ButtonRow = ({ onUpdateUserInfoButtonClick, disabled }) => { ...

const handleCancel = () =>  ...
-> const handleCancelUpdateUserButtonClick = () => ...
```

- 추상적인 이름보다 , 보다 더 상세한 이름으로 어떤 동작을 하는지 정확하게 알 수 있습니다. (onClick x)
- 변수의 의도가 더 확실히 드러나서 이해하기가 더 쉬워집니다.
- 일관성있는 변수명을 사용합니다.


## 2. 함수

[RPD-1948](https://github.com/twinnylab/taras-web/pull/72)

```js
const handleDestinationChange = (stationName, stationGroupId, destinationIndex) => {
// before
const handleDestinationChange = ({ stationName, stationGroupId, destinationIndex }) => {
// after
```

[RPD-1950](https://github.com/twinnylab/taras-web/pull/71)

```js
export const memberUtil = (selectedMemberGrade, registeredMembers, members, managers, administrators) => {
// before
export const memberUtil = ({ selectedMemberGrade, registeredMembers, members, managers, administrators }) => {
// after
```

[RPD-1951](https://github.com/twinnylab/taras-web/pull/73)

```js
const getStartedAt = (createdAt, serviceId, serviceNumber, phase, recallService) => (
// before
const getStartedAt = ({ createdAt, serviceId, serviceNumber, phase, recallService }) => (
// after
```

- 매개변수의 개수가 너무 많은 경우 , 테스트 해야하는 경우가 늘어나고 각각 다른 인수들로 여러 경우를 테스트해야하는 경우가 생깁니다.
- 2개가 이상적이고 , 그 이상으로 간 경우에는 통합되거나 객체를 이용할 수 있습니다.


