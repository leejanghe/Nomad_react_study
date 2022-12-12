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

<br />

### theme

theme은 색상을 객체로 관리 할 수 있는 유용한 함수다. 셋팅을 할 때 index에서 themeProvieder를 설정 후 app컴포넌트에 감싸준다. 그 후 적용하고 싶은 색상 코드를 객체에 담아 주면 된다.

```js
import React from "react";
import ReactDOM from "react-dom/client";
import { ThemeProvider } from "styled-components";
import App from "./App";

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
};

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <ThemeProvider theme={darkTheme}>
    <App />
  </ThemeProvider>
);
```

셋팅 후 props를 활용해서 색상을 꺼내서 사용하면 된다.

```js
import styled from "styled-components";

const Title = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
  background-color: ${(props) => props.theme.backgroundColor};
`;

function App() {
  return (
    <Wrapper>
      <Title>Hello</Title>
    </Wrapper>
  );
}

export default App;
```
