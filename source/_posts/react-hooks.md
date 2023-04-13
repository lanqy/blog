
`forwardRef` 是 React 中的一个 API，用于在函数组件中向子组件传递 `ref`。通过使用 `forwardRef`，可以将 `ref` 传递给函数组件内部的子组件，从而让子组件可以访问到父组件传递的 `ref`。下面是一个使用 `forwardRef` 的示例代码：

```js
const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

在上面的代码中，`MyInput` 组件使用 `forwardRef` 声明，以便可以接收父组件传递的 `ref`。`MyInput` 组件内部将 `ref` 传递给 `<input>` 元素，从而让父组件可以访问到 `<input>` 元素的 DOM 节点。在 `Form` 组件中，我们使用 `useRef` 创建了一个 `inputRef`，并将其传递给 `MyInput` 组件。在 `handleClick` 函数中，我们可以通过 `inputRef.current.focus()` 来聚焦到 `<input>` 元素。

更多关于 `forwardRef` 的信息可以参考 React 官方文档：

https://reactjs.org/docs/forwarding-refs.html

https://beta.reactjs.org/docs/forwarding-refs.html


React 的 forwardRef 可以用来转发（forward） ref 给函数式组件内部的 DOM 元素或子组件。以下是一个示例：

```js
import React, { forwardRef } from 'react';

const MyButton = forwardRef((props, ref) => {
  return (
    <button ref={ref} className="my-button">
      {props.children}
    </button>
  );
});

function App() {
  const buttonRef = useRef(null);

  const handleClick = () => {
    buttonRef.current.focus();
  };

  return (
    <div>
      <MyButton ref={buttonRef}>Click me</MyButton>
      <button onClick={handleClick}>Focus the button</button>
    </div>
  );
}
```

在上面的示例中，MyButton 是一个函数式组件，通过 forwardRef 将 ref 转发给 <button> 元素。在 App 组件中，我们可以使用 useRef 创建一个 ref，并将其传递给 MyButton 组件。然后，我们可以在 handleClick 函数中调用 buttonRef.current.focus() 来聚焦这个按钮。

以下是 React 中常用的 Hooks 和示例：

useState 用于在函数组件中声明和更新 state。
```js
import React, { useState } from 'react';

function Counter() {
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
useEffect 用于在组件渲染后执行副作用操作，如订阅和取消订阅事件、请求数据等。
```js
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function User({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    async function getUser() {
      const response = await axios.get(`https://jsonplaceholder.typicode.com/users/${userId}`);
      setUser(response.data);
    }

    getUser();
  }, [userId]);

  if (!user) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
}

```
useContext 用于从父组件传递下来的 context 中获取值。
以下是使用React的useContext钩子的示例：

```js
import React, { createContext, useContext } from 'react';

// 创建一个context
const ThemeContext = createContext('light');

// 使用context的组件
function MyComponent() {
  // 从context中读取值
  const theme = useContext(ThemeContext);

  return (
    <div className={`App ${theme}`}>
      <h1>Hello, World!</h1>
    </div>
  );
}

// 渲染组件
function App() {
  return (
    // 提供context的值
    <ThemeContext.Provider value="dark">
      <MyComponent />
    </ThemeContext.Provider>
  );
}

export default App;
```

在上面的示例中，我们创建了一个名为`ThemeContext`的context，并将其值设置为`light`。然后，我们创建了一个名为`MyComponent`的组件，并使用`useContext`钩子从`ThemeContext`中读取值。最后，我们在`App`组件中提供了`ThemeContext`的值，并渲染了`MyComponent`组件。


useRef 用于在函数组件中引用 DOM 元素或保存变量的值，避免因为重新渲染导致变量值丢失。
```js
import React, { useState, useRef } from 'react';

function InputWithFocusButton() {
  const inputEl = useRef(null);
  const [value, setValue] = useState('');

  function handleClick() {
    inputEl.current.focus();
  }

  return (
    <div>
      <input type="text" ref={inputEl} value={value} onChange={(e) => setValue(e.target.value)} />
      <button onClick={handleClick}>Focus the input</button>
    </div>
  );
}

```
useCallback 用于缓存回调函数，避免因为重新渲染导致相同的回调函数重复创建。
```js
import React, { useState, useCallback } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={handleIncrement}>
        Click me
      </button>
    </div>
  );
}

```
useMemo 用于缓存计算结果，避免因为重新渲染导致重复计算。
```js
import React, { useMemo } from 'react';

function ExpensiveCalculation({ num }) {
  const result = useMemo(() => {
    console.log('Calculating...');
    let sum = 0;
    for (let i = 1; i <= num; i++) {
      sum += i;
    }
    return sum;
  }, [num]);

  return <div>{result}</div>;
}

```
useReducer 用于管理复杂的 state，类似于 Redux 中的 reducer。
```js
import React, { useReducer } from 'react';

function Counter() {
  const [state, dispatch] = useReducer((prevState, action) => {
    switch (action.type) {
      case 'increment':
        return { count: prevState.count + 1 };
      case 'decrement':
        return { count: prevState.count - 1 };
      default:
        throw new Error();
    }
  }, { count: 0 });

  return (
    <div>
      <p>You clicked {state.count} times</p>
      <button onClick={() => dispatch({ type: 'increment' })}>
        Click me
      </button>
      <button onClick={() => dispatch({ type: 'decrement' })}>
        Click me
      </button>
    </div>

```