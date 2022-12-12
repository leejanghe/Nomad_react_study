## 간단히 기록하기 복기하는 마음으로

---

## 스타일 컴포넌트

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
