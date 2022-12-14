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

### Option Props

bgColor는 꼭 필요한 Props인데, 그렇다면 반드시 필요하지 않은 Props는 어떻게 설정할까요?
그냥 물음표를 붙이면 된다.

```js
interface CircleProps {
  bgColor: string;
  borderColor?: string;
}
```

borderColor는 타입이 `string | undefined` 가 됩니다.

이렇게 | 연산자를 이용해 타입을 여러 개 연결하는 방식을 유니온 타입이라고 부릅니다.

해당 값은 `string` 또는 `undefined`니까 값이 `undefined`더라도 당황하지 말고 넘어가세요! 하고 타입스크립트에게 말해주는 거죠.

우리는 지금 CircleProps와 ContainerProps 두 개가 있는데, CircleProps는 선택값으로 받되 ContainerProps에서는 필수값으로 받아봅시다.

반드시 border값이 있어야 하긴 하는데, 그 값이 부모에서 넘어올 수도 있고 아닐 수도 있는 상황이란 거죠!

```js
interface CircleProps {
  bgColor: string;
  borderColor?: string;
}

interface ContainerProps {
  bgColor: string;
  borderColor: string;
}
```

그래서 만약 부모(Circle)로부터 넘겨받은 값이 없다면 그냥 bgColor 값이 borderColor 값이 되도록 합니다.

`borderColor={borderColor ?? bgColor}`

- nullish 병합 연산자

* `a ?? b에서 a 가 null또는 undefined라면 b, 아니라면 a`

```js
// ...
const Container =
  styled.div <
  ContainerProps >
  `
  width: 300px;
  height: 300px;
  background-color: ${(props) => props.bgColor};
  border: 3px solid ${(props) => props.borderColor};
  border-radius: 50%;
`;

function Circle({ bgColor, borderColor }: CircleProps) {
  return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor} />;
}
```

또는 { text = “Lorem Ipsum”} 과 같이 기본값을 설정해 줄 수도 있겠죠!

```js
/* Circle.tsx */

import styled from "styled-components";
interface CircleProps {
  bgColor: string;
  borderColor?: string;
  text?: string;
}
interface ContainerProps {
  bgColor: string;
  borderColor: string;
}

const Container =
  styled.div <
  ContainerProps >
  `
  width: 300px;
  height: 300px;
  background-color: ${(props) => props.bgColor};
  border: 3px solid ${(props) => props.borderColor};
  border-radius: 50%;
`;

function Circle({ bgColor, borderColor, text = "Lorem Ipsum" }: CircleProps) {
  return (
    <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
      {text}
    </Container>
  );
}

export default Circle;
```

<br />

### State

타입스크립트를 사용하고 있다면 리액트의 state 초기값을 보고 알아서 타입을 유추해 줍니다.

`const [value, setValue] = useState(true);` 라면 Boolean 값이 여기 들어오겠거니 알아챕니다

만약 state값이 undefined나 null이 될 수 있는 등, 여러 타입이 올 수 있다면 따로 타입을 지정해 줄 수 있습니다.

`const [value, setValue] = useState<number | null>(0);`

<br />

### form event

onChange 는 Form Events 임을 알 수 있습니다.

그리고 이벤트를 발생시킬 요소는 `<HTMLInputElement>`입니다.

따라서 e 객체의 타입을 `React.FormEvent<HTMLInputElement>` 로 지정합니다.

```js
import React, { useState } from "react";

function App() {
  const [value, setValue] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    const {
      currentTarget: { value },
    } = event;
    setValue(value);
  };
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("hello", value);
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={onChange}
          type="text"
          placeholder="username"
        />
        <button>Log in</button>
      </form>
    </div>
  );
}
export default App;
```

<br />

## Theme

스타일드 컴포넌트를 위한 타입스크립트 정의는 declarations 파일을 통해 확장할 수 있습니다.
styled.d.ts 란 이름으로 declarations 파일을 생성 후 아래 내용을 붙여넣습니다.
(d.ts 파일은 선언(delcaration)을 통해 타입스크립트 코드의 타입 추론을 돕는 파일입니다.)
스타일드 컴포넌트의 DefaultTheme 에게 인터페이스를 지정해주는 거죠.
[참고문서](https://styled-components.com/docs/api#typescript)

```js
/* styled.d.ts */

// import original module declarations
import 'styled-components';

// and extend them!
declare module 'styled-components' {
  export interface DefaultTheme {
    textColor: string;
	bgColor: string
  }
}
```

import { DefaultTheme } 으로 가져오고, 작성한 테마의 타입으로 지정해준 다음 export 합니다.

```js
/* theme.ts */

import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
  textColor: "#000",
  bgColor: "#fff",
};

export const darkTheme: DefaultTheme = {
  textColor: "#fff",
  bgColor: "#000",
};
```

테마를 적용할 컴포넌트를 <ThemeProvider/> 로 감싸고 theme={}로 지정해주면 되겠죠!

```js
/* index.tsx */

//...
import { ThemeProvider } from "styled-components";
import { darkTheme } from "./theme";

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

그러면 props.theme로 가져올 수 있게 되며, 어떤 속성을 어떤 타입으로 써야하는지 명확히 알 수 있습니다.
