# 2025 前端測試

## 1

There is an array, each item has such format:

```
{firstName: 'xxx', lastName: 'xxx', customerID: 'xxx', note: 'xxx', profession: ‘xxx’}
```

lastName, note can be empty, customerID can only be a set of digital numbers.
profession can only have ‘student’, ‘freelancer’, ‘productOwner’, ‘engineer’ or
‘systemAnalytics’.

### Q1

Please follow the principle (‘firstName’ + ‘lastName’ + ‘customerID’) to sort this
array and print it out.

```
function sortUsersName(users) {
  const combineCompareString = (user) =>
    `${user.firstName}${user.lastName || ""}${user.customerID}`;

  return users.sort((a, b) =>
    combineCompareString(a).localeCompare(combineCompareString(b))
  );
}

console.log(sortUsersName(users));
```

### Q2

Please sort by ‘profession’ to follow the principle.
(‘systemAnalytics’ > ‘engineer’ > ‘productOwner’ > ‘freelancer’ > ‘student’’)

```
function sortByType(users) {
  const typeOrder = {
    systemAnalytics: 0,
    engineer: 1,
    productOwner: 2,
    freelancer: 3,
    student: 4,
  };

  return users.sort(
    (a, b) => typeOrder[a.profession] - typeOrder[b.profession]
  );
}
```



# 2

```
<div class="container">
  <div class="header">5/8 外出確認表</div>
  <div class="content">
    <ol class="shop-list">
      <li class="item">麵包</li>
      <li class="item">短袖衣服</li>
      <li class="item">飲用水</li>
      <li class="item">帳篷</li>
    </ol>
    <ul class="shop-list">
      <li class="item">暈車藥</li>
      <li class="item">感冒藥</li>
      <li class="item">丹木斯</li>
      <li class="item">咳嗽糖漿</li>
    </ul>
  </div>
  <div class="footer">以上僅共參考</div>
</div>
```

```
/** CSS **/

.container {
  font-size: 14px;
}
.container .header {
  font-size: 18px;
}
  .container .shop-list {
  list-style: none;
  margin-left: -15px;
}
.container .shop-list li.item {
  color: green;
  }
.container .shop-list .item {
    color: blue;
    /* Q1. Explain why does this color not works, and how to fix make it work on 1st list */

    /* Answer: this specificity of the selector is lower than the specificity of the selector .container .shop-list li.item.
    To fix it, we can increase the specificity of the selector by change the selector to .container ol.shop-list li.item of .container .shop-list:first-child li.item */
}

/*Q2. Write styling make every other line give background color to next one */
.container .shop-list li:nth-child(odd) {
  background-color: #a1a6ff;
}
```



## 3

Please write down a function to console log unique value from this array.

```
let items = [1, 1, 1, 5, 2, 3, 4, 3, 3, 3, 3, 3, 3, 7, 8, 5, 4, 9, 0, 1, 3, 2, 6, 7, 5, 4, 4, 7, 8, 8, 0, 1, 2, 3, 1];
```

Ans:

```
function getUniqueNumber(items) {
  return [...new Set(items)];
}
```



## 4

What is virtual DOM and what purpose does it aim to solve?

Ans: Manipulating and comparing the actual DOM consumes a significant amount of resources. Therefore, Facebook developed the Virtual DOM (VDOM), a simulated DOM tree. In React, every state change actually modifies the VDOM. React then compares the differences before and after the change and synchronizes the VDOM with the real DOM in the most resource-efficient manner.



## 5

Can you explain about the type of never and what is the differ with void?

Ans: In TypeScript's function type inference, void represents the absence of a return value, while never indicates that there will never be a return value (such as in an infinite loop or when an error is thrown).



## 6

What is difference between framework base website and normal website (none
framework)

Ans: The advantage of using a framework lies in its extensive infrastructure, such as Router, Code Splitting, and multi-language support, among other features. If you choose not to use a framework, you would need to handle these requirements on your own. Therefore, if it's just a single-page application, not using a framework might suffice to meet the needs effectively. However, for developing a website with multiple pages, opting out of a framework would require integrating various libraries and configuring them separately. In contrast, a framework offers a more standardized and easier-to-start development experience.

## 7

Read the code below, please figure out why after “Switch Person” button clicked, the
tasks state doesn’t update correctly, and how to make it update as we expected


```
import { useState } from 'react';
export default function TaskManager() {
const [isPersonAlice, setIsPersonAlice] = useState(true);
return (
  <div>
  {isPersonAlice ? (
    <TaskCounter name="Alice" />
  ) : (
    <TaskCounter name="Bob" />
  )}
  <button onClick={() => {
    setIsPersonAlice(!isPersonAlice);
  }}>
  Switch Person
  </button>
  </div>
);
}
function TaskCounter({ name }) {
const [tasks, setTasks] = useState(0);
return (
<>
  <h1>{name}'s tasks: {tasks}</h1>
  <button onClick={() => setTasks(tasks + 1)}>
  Complete Task
  </button>
</>
);
}
```

Ans: The transformation of isPersonAlice for VDOM means that TaskCounter is updated with new props rather than removing the original TaskCounter and creating a new one. To resolve this issue, you need to add a key prop to TaskCounter and assign it the value of name (assuming name is unique), which will solve the problem.

```
import { useState } from 'react';
export default function TaskManager() {
const [isPersonAlice, setIsPersonAlice] = useState(true);
return (
  <div>
  {isPersonAlice ? (
    <TaskCounter name="Alice" key="Alice" />
  ) : (
    <TaskCounter name="Bob" key="Bob" />
  )}
  <button onClick={() => {
    setIsPersonAlice(!isPersonAlice);
  }}>
  Switch Person
  </button>
  </div>
);
}
function TaskCounter({ name }) {
const [tasks, setTasks] = useState(0);
return (
<>
  <h1>{name}'s tasks: {tasks}</h1>
  <button onClick={() => setTasks(tasks + 1)}>
  Complete Task
  </button>
</>
);
}
```


## 8

Read about the code below, please describe the issues and how you will be going to improve it

```
const TodoList = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: '學習 React', completed: false, studyPoint: 3 },
    { id: 2, text: '建立專案', completed: false, studyPoint: 1 }
  ]);
  const { id, text, studyPoint } = todos;
  const [basePoints, setbasePoints] = useState(3);
  const [sumPoints, setSumPoints] = useState(0);
  const toggleTodo = (id) => {
    const todo = todos.find(t => t.id === id);
    todo.completed = !todo.completed;
    setTodos(todos);
  };
  const handleStudyPointsChange = (e) => {
    basePoints(e.target.value);
    setSumPoints(parseInt(value1) + parseInt(e.target.value));
  };
  return (
    <div>
    <p>課程名稱: {text}</p>
    <label>學習點數: </label>
    <input type="number" value={studyPoint} onChange={handleStudyPointsChange} />
    <p>總累積點數: {sumPoints}</p>
    <button onClick={toggleTodo(id)}>篩選課程</button>
    </div>
  );
}
```

- `<button onClick={toggleTodo(id)}>篩選課程</button>` 應改為 `<button onClick={() => toggleTodo(id)}>篩選課程</button>`
- handleStudyPointsChange 內的 `basePoints(e.target.value);` 應改為 `setbasePoints(e.target.value)`;
- `setbasePoints` 應改為 `setBasePoints` 以維持命名規則一致性
- `const { id, text, studyPoint } = todos;`這行解構無效，應改為 `const { id, text, studyPoint } = todos[0];`，或是新增一個 state 來控制當前顯示的 todo
- 假設 總累積點數 是顯示 todos 內的 todo studyPoint 的總和，則建議將 sumPoints 及 setSumPoints 移除，使用計算的形式取代
- handleStudyPointsChange 改成直接修改 todos 的內容，並且使用 useCallback 包裹
- toggleTodo 的操作應該在 setTodos 內完成

以下為修改後的程式碼

```
import { useState, useMemo, useCallback } from "react";

export const TodoList = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: "學習 React", completed: false, studyPoint: 3 },
    { id: 2, text: "建立專案", completed: false, studyPoint: 1 },
  ]);
  const { id, text, studyPoint } = todos[0];

  const toggleTodo = (id) => {
    setTodos((prevTodos) => {
      const targetIndex = prevTodos.findIndex((t) => t.id === id);
      if (targetIndex !== -1) {
        const newTodos = [...prevTodos];
        newTodos[targetIndex] = {
          ...prevTodos[targetIndex],
          completed: !prevTodos[targetIndex].completed,
        };

        return newTodos;
      }
      return prevTodos;
    });
  };

  const handleStudyPointsChange = useCallback((e) => {
    let value = parseInt(e.target.value || "0");

    setTodos((prevTodos) => {
      const targetIndex = todos.findIndex((todo) => todo.id === id);
      if (targetIndex !== -1) {
        const newTodo = { ...todos[targetIndex], studyPoint: value };
        let newTodos = [...prevTodos];
        newTodos[targetIndex] = newTodo;
        return newTodos;
      } else {
        return prevTodos;
      }
    });
  }, []);

  const sumPoints = useMemo(() => {
    return todos.reduce((acc, val) => acc + val.studyPoint, 0);
  }, [todos]);

  return (
    <div>
      <p>課程名稱: {text}</p>
      <label>學習點數: </label>
      <input
        type="number"
        value={studyPoint}
        onChange={handleStudyPointsChange}
      />
      <p>總累積點數: {sumPoints}</p>
      <button onClick={() => toggleTodo(id)}>篩選課程</button>
    </div>
  );
};

```



## 9

Read about the code below, suggest how to improve the code

```
function ParentComponent() {
  const [name, setName] = useState("Naro");
  const [age, setAge] = useState(12);
  return (
    <div>
      <ChildComponent name={name} age={age} />
      <GrandchildComponent name={name} age={age} />
    </div>
  );
}
function ChildComponent({ name, age }) {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <GrandchildComponent name={name} age={age} />
    </div>
  );
}
function GrandchildComponent({ name, age }) {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
}
```

Ans: use useContext method

```

const PersonCtx = createContext({
  name: "",
  age: 0,
  setName: () => {},
  setAge: () => {}
})

const usePersonCtx = useContext(PersonCtx);


function ParentComponent() {
  const [name, setName] = useState("Naro");
  const [age, setAge] = useState(12);
  return (
    <PersonCtx.Provider value={{
      name,
      setName,
      age,
      setAge
    }}>
      <div>
        <ChildComponent />
        <GrandchildComponent />
      </div>
    </PersonCtx.Provider>
  );
}
function ChildComponent() {
  const { name, age } = usePersonCtx()
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <GrandchildComponent />
    </div>
  );
}
function GrandchildComponent() {
  const { name, age } = usePersonCtx()
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
}

```



## 10.

Read about the code below, achieving that make input element in “SearchInput” to be
focused while search button on click

```
function SearchButton() {
  return (
    <button> Search </button>
  );
}
function SearchInput() {
  return (
    <input/>
  );
}
export default function Page() {
  return (
  <>
    <nav>
     <SearchButton />
    </nav>
    <SearchInput />
  </>
  );
}
```

Anser: The simplest way is to assign an ID to the input, and when the SearchButton is pressed, locate the input using that ID and set it to focus.

```
function SearchButton() {
  const handleSearchClick = useCallback(()=> {
    const target = document.querySelector("#search");
    if(target) {
      target.focus();
    }
  }, [])
  return (
    <button> Search </button>
  );
}
function SearchInput() {
  return (
    <input id="search"/>
  );
}
export default function Page() {
  return (
  <>
    <nav>
     <SearchButton />
    </nav>
    <SearchInput />
  </>
  );
}
```

Of course, you can also pass the Ref of the input through methods like useRef, but for the sake of simplicity, we won't demonstrate that here.
