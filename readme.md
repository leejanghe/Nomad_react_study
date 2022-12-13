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

<br />

## 타입스크립트

### 타입스크립트란 ?

자바스크립트는 너무나 관대(?)해서 오냐오냐 하고 넘어갈 때가 많습니다.

const sum = (a,b) => a+b; 에서 a와 b는 숫자로 받을 생각이었지만, 여기에 문자열을 넘기면 그대로 적용이 됩니다. 또한 존재하지 않는 프로퍼티를 읽어 undefined를 출력하기도 합니다.

그래서 좀더 깐깐한 언어인 타입스크립트를 통해 적용 할 수 있습니다. 타입스크립트는 자바스크립트를 바탕으로 하여 거의 유사하지만 좀 더 강력한 기능을 제공한다.

### Type Annotation

```js
// Type Annotation
const sum = (a: number, b: number) => a + b;
sum(1, 2);
```

코드 타입을 정할때 : 를 이용해서 표현합니다.

<br />

### Props설정

아래과 같이 `<Circle/>`컴포넌트에 프롭스가 필요하다 가정을 해보자

```js
/* App.tsx */

import Circle from "./Circle";

function App() {
  return (
    <>
      <Circle bgColor="orange" />
      <Circle bgColor="green" />
    </>
  );
}

export default App;
```

interface 키워드를 이용해 인터페이스를 만들어 줄 수 있습니다.
이때 인터페이스 이름은 I를 붙이기도 합니다 (ex. 가격정보 → IPriceData)
그럼 인터페이스를 만들어 보죠. <Circle/>에게 넘어오는 각 props의 타입은 CircleProps라고 알려줍시다.

```js
import styled from "styled-components";

const Container = styled.div``;

interface CircleProps {
  bgColor: string;
}

function Circle({ bgColor }: CircleProps) {
  return <Container />;
}

export default Circle;
```

<Circle/>이 넘겨받은 bgColor 란 props를 다시 자식 컴포넌트인 <Container/>에게 넘겨줄 차례입니다.

스타일드 컴포넌트인 <Container/> 가 받을 props들도 설정해준 뒤, styled.div<{인터페이스명}>``; 형태로 사용합니다.

추가적으로 이렇게 <> 형태로 사용하는 것을 제네릭(Generic)이라 하는데, 타입정의를 매개변수로 넘겨주는 것처럼 사용하고 있습니다.

<br />
