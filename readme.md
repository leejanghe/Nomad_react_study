## 간단히 기록하기 복기하는 마음으로

---

## 스타일 컴포넌트 (props & fnc)

1. prop를 활용해서 재사용하기
2. 기존 컴포넌트의 모든 속성을 가져와서 확장

```js
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

// 재사용 예시
const Box = styled.div`
  background-color: ${(props) => props.bgColor};
  width: 100px;
  height: 100px;
`;

// 확장의 예시
const Circle = styled(Box)`
  border-radius: 50px;
`;

function App() {
  return (
    <Father>
      <Box bgColor="teal" />
      <Circle bgColor="tomato" />
    </Father>
  );
}

export default App;
```

<br />

### as와 attrs활용

1. as는 html태그를 변경해줄수 있다. 예를들어 버튼을 링크로표현 하고 싶을 때 컴포넌트에 `as="a"` 작성
2. attrs는 객체에 속성값을 넣을 수 있다. 중복을 줄일수 있는 장점이있다.

```js
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const Input = styled.input.attrs({ required: true })`
  background-color: tomato;
`;

function App() {
  return (
    <Father as="header">
      <Input />
      <Input />
      <Input />
      <Input />
      <Input />
    </Father>
  );
}

export default App;
```
