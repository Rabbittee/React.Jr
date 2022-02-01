---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# Welcome to React Jr.

React for the junior

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/ashramwen" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    Augustine <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Intro

React 是用來實作 UI 的 JavaScript 函式庫

- 📝 **Declarative** - 你只要在你的應用程式中為每個情境設計一個簡單的 view，React 就會在資料變更時有效率的自動更新並 render 有異動的元件
- 🎨 **Component-Based** - 建置經過封裝過而獨立的元件，管理 state 組合成複雜的使用者介面
- 🧑‍💻 **JSX** - 使用 JSX 語法結合 HTML 和 JS
- 🤹 **Props** - Parent 單向傳遞給 Children 的資料
- 🎥 **State** - 由各 Component 管理且 private 
- 🛠 **Hooks** - ver 16.8 後引入，提供 function component 使用 state 及其他功能  

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Navigation

### Keyboard Shortcuts

|     |     |
| --- | --- |
| <kbd>right</kbd> / <kbd>space</kbd>| 下一個動畫或是頁面 |
| <kbd>left</kbd>  / <kbd>shift</kbd><kbd>space</kbd> | 上一個動畫或是頁面 |
| <kbd>up</kbd> | 上一頁 |
| <kbd>down</kbd> | 下一頁 |

<!-- https://sli.dev/guide/animations.html#click-animations -->
<img
  v-click
  class="absolute -bottom-9 -left-7 w-80 opacity-50"
  src="https://sli.dev/assets/arrow-bottom-left.svg"
/>
<p v-after class="absolute bottom-23 left-40 opacity-30 transform -rotate-10">控制選單 !</p>

---

# Root and App

React 會有一個最根本的入口，用來宣染整個 React web app，習慣性會將一些 middleware 放在這

如果是 CRA，入口會是 index.js，而使用 Vite 會是 main.jsx，內容大致如下

```js
// index/main
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
)
```

---

# App

除了入口點以外，還會有一個最底層的 Component，通常我們會用來放置 global components 或是 configuration 等等
通常檔案名是 App.{js,jsx,tsx}

以下是 App.jsx 示意

```jsx
function App() {
  useInit();

  return (
    <>
      <Header />
      <Main />
      <Dialog />
    </>
  );
}

export default App;
```

---

# Function component

自從 Hooks 被引入後，Function component 成為了 React 的主流
簡單乾淨的特性，讓工程師能夠更容易地設計 Component

```js {all|2}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

<p v-after>使用 JSX 語法並透過 return 來 render component</p>

---

# Render 一個 Component

在上一頁我們建立了一個 Welcome component

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

接下來我們就可以將在 Component 在任意元件上使用

```jsx
function App() {
  return (
    <Header />
    <Welcome name="Sara" />
    <Footer />
  );
}
```

---

# Props

Props 是 Parent 和 Children 溝通的資料，從 Parent 單方向傳遞給 Children。

```jsx {all|2,8}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <Header />
    <Welcome name="Sara" />
    <Footer />
  );
}
```

<p v-click="1">

App 透過 `name` 將 `Sara` 傳遞給 Welcome，Welcome 則能夠透過 props.name 來取得 `Sara`

</p>

<p v-after>
要注意的是，props 是唯讀的。
</p>

---

# 組合 Component

Component 可以在輸出中引用其他 component。

舉例來說，我們可以建立一個 render 多次 Welcome 的 App component：

```jsx {4-6}
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Tony" />
      <Welcome name="Eddie" />
    </div>
  );
}
```

---

# State and UseState {1}

State 類似於 props，但是它是各 Component 所獨自擁有，並且是 private

早期的 Function component 是 stateless 的，但是隨著 Hooks 的引用，現在 Function component 也能擁有 State

###### State Hook 範例

```jsx
import React, { useState } from 'react';

function Example() {
  // 宣告一個新的 state 變數，我們叫他「count」
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

---

# State and UseState {2}

```jsx {3|3,5,10}
function Example() {
  // 宣告一個新的 state 變數，我們叫他「count」
  const [count, setCount] = useState(0);

  const handleClick = () => setCount(count + 1);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={handleClick}>
        Click me
      </button>
    </div>
  );
}
```

useState 是一個 hook，我們可以使用它來建立 local state，<mark> 即使在 re-render 之後仍會保留這些 state </mark>。

<p v-click="1">
useState 回傳一組數值：[目前 state 值, 更新 state 用 function]。
</p>
<p v-after>

useState 唯一的 argument 是初始狀態。在上面的例子中，他是 0 因為我們希望計數器從零開始。
<br>
如果沒有給予初始狀態，將會是 `undefined`。

</p>

<style>
  mark {
    background-color: coral;
  }
</style>

---

# 宣告多個 state 變數

可以在一個 component 中使用 State Hook 不只一次

```jsx
function ExampleWithManyStates() {
  // 宣告多個 state 變數!
  const [age, setAge] = useState(18);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...

  return ...
}
```

---

# Re-render

Re-render 是 React 一個很重要的機制，透過 Re-render 將能夠重新繪製我們所期望的畫面。

<div v-click="1">
有三種情況會讓 React re-render 我們設計的 Component:
</div>

<p v-click="2">

- State 更新
- Props 更新
- Parent re-render

</p>

---
layout: center
class: text-center
---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
