# redux-tookit

- redux를 사용하다보면 boilerplate 코드라고 하는 계속해서 반복하는 코드를 작성하게 된다. 이것을 해결하기 위해 redux가 제안한 것이 redux-tookit이다.

## 1. 패키지 설치

```zsh
yarn add @reduxjs/tookit
```

---

## 2. createAction

1. redux 사용할 때

```js

// action type strings
const ADD = "ADD";
const DELETE = "DELETE";

// action creator functions
const addToDo = (text) => {
  return {
    type: ADD,
    text,
  };
};

const deleteToDo = (id) => {
  return {
    type: DELETE,
    id: parseInt(id),
  };
};

const reducer = (state = [], action) => {
switch (action.type) {
  case ADD:
    return [{ text: action.text, id: Date.now() }, ...state];
  case DELETE:
    return state.filter(toDo => toDo.id !== action.id);
  default:
    return state;
}
```

2. redux-tookit 사용할 때

- Defining Action Creators with `createAction` : action type과 payload를 만들어준다.

```js

const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

console.log(addToDo, deleteToDo); // f ADD, f DELETE (function)
console.log(addToDo.type, deleteToDo.type); // ADD, DELETE (string)
console.log(addToDo.toString, deleteToDo.toString); // ADD, DELETE (string) -> action type만 반환하도록 override 되어있다.
console.log(addToDo(), deleteToDo());
// createAction으로 만든 액션을 호출하면 자동으로 type과 payload를 생성해준다.
// {type : "ADD", payload : undefined}, {type : "DELETE", payload : undefined}


// action creator 자체를 action type으로 참조한다.
const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      return [{ text: action.payload, id: Date.now() }, ...state];
    case deleteToDo.type:
      return state.filter(toDo => toDo.id !== action.payload);
    default:
      return state;
}
```

---

## 3. createReducer

1. 그냥 reducer

- initialState를 action을 통해 새로운 state로 변형시키고, 그 값을 store에 전달한다.
- `switch statements`와 `immutable` updates를 사용하는 일반적인 reducer
  ```js
    const reducer = (state = [], action) => {
      switch (action.type) {
        case addToDo.type:
          return [{ text: action.payload, id: Date.now() }, ...state];
        case deleteToDo.type:
          return state.filter(toDo => toDo.id !== action.payload);
        default:
          return state;
    }
  ```

2. creatorReducer()

- 첫 번째 인자로는 initialState,
- 두 번째 인자로는 특정 action type을 처리하는 reducer case를 객체로 매핑한 게 들어간다.
  - `createAction`으로 만들어진 action creators는 `computed property syntax`를 사용하면 객체의 key 값을 `action type`으로 직접 사용할 수 있다.
    ```js
    // Computed property
      [keyName] : (state, action) => { }
    ```
- 장점 1) 불변성 관리를 자동으로 해준다.
  - 내부적으로 immer 라이브러리를 사용하고 있어서 `mutative` code를 작성해도 state를 업데이트 할 수 있다. 즉, 리듀서 내에서 배열에 push 메소드를 사용하여 기존 배열의 값을 변환하는 코드를 작성할 수 있다.
  - 실제로 reducer는 모든 상태 변화를 동일한 `복사물로 변환하는 대체 상태`를 받는다.
- 장점 2) switch문으로 case를 나누지 않고도 reducer를 작성할 수 있다.
  ```js
  const reducer = createReducer([], {
    [addToDo]: (state, action) => {
      state.push({ text: action.payload, id: Date.now() });
    },
    [deleteToDo]: (state, action) =>
      state.filter((toDo) => toDo.id !== action.payload),
  });
  ```

---

## 3. createSlice

- 작성한 reducer 함수의 이름을 기반으로 action type, action creator를 자동으로 만들어준다.

  ```js
  const toDos = createSlice({
    name: "toDosReducer",
    initialState: [],
    reducers: {
      add: (state, action) => {
        state.push({ text: action.payload, id: Date.now() });
      },
      remove: (state, action) =>
        state.filter((toDo) => toDo.id !== action.payload),
    },
  });

  export const { add, remove } = toDos.actions;

  console.log(add, remove);
  // {type : "todosReducer/ADD", payload : undefined}, {type : "todosReducer/REMOVE", payload : undefined}

  export default configureStore({ reducer: toDos.reducer });
  ```
