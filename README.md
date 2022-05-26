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

</br>

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
- 실제로 인자가 너무 많아지니까 무슨 인자를 넣었는지 , 무슨 인자를 빼먹었는지 헷갈리는 경우가 많았던 것 같습니다.


[RPD-2000](https://github.com/twinnylab/taras-web/pull/78)

```js
const getMenuItems = (role) => {
    return [
    { label: '강제 탈퇴', onClick: () => handlePopupOpen() },
    { label: '회원 등급 변경', onClick: () => handlePopupOpen(role) },
];

const handlePopupOpen = (role) => {
    ...
    const expelDescription = ( ...
    const toMemberDescription = ( ...
    const toManagerDescription = ( ...
    ...
}
```

handlePopupOpen 함수 안에서 3가지 함수가 작동되는 경우입니다. 이 경우를

```js
const menuItems = [
    { label: '강제 탈퇴', onClick: handleExpelFromWorkpsaceButtonClick },
    { label: '회원 등급 변경', onClick: handleChangeRoleButtonClick },
];

const handleChangeRoleButtonClick = () => { ...
const handleExpelFromWorkpsaceButtonClick = () => { ...
```

각각 함수에서 1가지의 일만 하도록 나누었습니다. 만약 함수가 여러개의 행동을 가진 경우 작성 및 테스트가 어려워집니다. 한가지의 행동을 하도록 한다면 , 좀 더 고치기 쉽고 읽기 쉬워집니다. 실제로 함수 하나에 여러 함수들을 모아놓은 뒤 분기하는 방법을 자주 사용했었는데 , 그렇게 되니까 어디서 어디로 이동하는 과정을 찾기 힘들었고 읽기 힘들었습니다.

[RPD-2041](https://github.com/twinnylab/taras-web/pull/83)

```js
const handleClick = ({ isChecked, data }) => (isChecked ? handleAddStation(data) : handleRemoveStation(data));
//before
const handleStationRowClick = ({ isChecked, data }) => {
    return isChecked ? handleAddStation(data) : handleRemoveStation(data);
};
//after


handleClick --> handleStationRowClick
```

클릭을 뜻하는 handleClick에서 handleStationRowClick 로 변경입니다. 범용적인 말보다 확실하게 어떤 일을 하는지 적어주는 것이 무슨 역할을 하는지 바로 이해할 수 있습니다.

```js
const isChekced = (data) => checkedStations.some((station) => station.id === data.id);
//before
const isCheckedStation = (data) => {
   return checkedStations.some((station) => station.id === data.id);
};
//after

isCheck --> isCheckedStation
```

역시 그냥 체크하는 것이 아니라 무엇을 체크하는지 확실하게 말해주는 것이 이해하기 쉽습니다.
- 변수명과 같은 맥락인데 , 변수는 보통 무엇인지 가르키는지 , 함수는 어떤 행동을 하는지를 생각하고 이름을 지으면 좋을 것 같습니다.

</br>

## 3. 중복 코드

```js
// before
let date = new Date(string);

  if (type === 'start') {
    date = new Date(date.setHours(0));
    date = new Date(date.setMinutes(0));
    date = new Date(date.setSeconds(0));
  } else {
    date = new Date(date.setHours(23));
    date = new Date(date.setMinutes(59));
    date = new Date(date.setSeconds(59));
  }
// after
let date = new Date(string);

  if (type === 'start') {
    date.setHours(0);
    date.setMinutes(0);
    date.setSeconds(0);
  } else {
    date.setHours(23);
    date.setMinutes(59);
    date.setSeconds(59);
  }
```
- date 객체가 계속 반복해서 생성되는 것을 줄였습니다.
- 중복된 것을 줄이는 것이 가독성 부분에서 굉장히 중요한 것 같습니다. 그런 김에 저희 joined 머시기 쿼리도 줄었으면 ,,,
- 계속 쓰는것은 귀찮을 뿐 아니라 반복해서 나오니까 헷갈리게 해서 집중력이 떨어지기도 하는 것 같습니다..

## 4. 매개 변수

```js
// before
 const numberFormat = (number) => {
    return number < 10 ? '0' + number : number;
  ...
  return {
    minutes: numberFormat(minutes),
    seconds: numberFormat(seconds),
}
// after
const getNumberFormatLessThenTen = (number) => {
    return '0' + number;
  };
  ...
  return {
      minutes: minutes < 10 ? getNumberFormatLessThenTen(minutes) : minutes,
    seconds: seconds < 10 ? getNumberFormatLessThenTen(seconds) : seconds,
}
```

- 매개변수가 들어와서 다시 나누어지는 경우 , 함수를 만들어서 나눈 경우입니다. 
- 이렇게 짜여진 자체가 한 가지 이상의 역할이며 , 나누는 것을 추천합니다.
- 함수로 나뉜 경우가 훨씬 읽기 쉽고 , 이해하기 쉬운 것 같습니다.
