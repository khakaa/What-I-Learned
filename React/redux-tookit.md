# redux-tookit

- redux를 사용하다보면 boilerplate 코드라고 하는 계속해서 반복하는 코드를 작성하게 된다. 이것을 해결하기 위해 redux가 제안한 것이 redux-tookit이다.

## 1. 패키지 설치

```zsh
yarn add @reduxjs/tookit
```

---

## 2. createAction

- Defining Action Creators with `createAction`

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

  ```js

  const addToDo = createAction("ADD");
  const deleteToDo = createAction("DELETE");

  console.log(addToDo, deleteToDo); // f ADD, f DELETE (function)
  console.log(addToDo.type, deleteToDo.type); // ADD, DELETE (string)
  console.log(addToDo(), deleteToDo());
  // {type : "ADD", payload : undefined}, {type : "DELETE", payload : undefined}


  // action creator 자체를 action type으로 참조한다.
  const reducer = (state = [], action) => {
    switch (action.type) {
    // switch문 에서 createAction으로 만든 action type을 써줄려면 뒤에 .type 이나 .toString()으로 문자열로 변환시켜줘야 한다.
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

2. creatorReducer

- 첫 번째 인자로는 initialState,
- 두 번째 인자로는 특정 action type을 처리하는 reducer case를 객체로 매핑한 게 들어간다.
  - `createAction`으로 만들어진 action creators는 `computed property syntax`를 사용하면 객체의 key 값을 직접적으로 사용할 수 있다.
- 내부적으로 immer 라이브러리를 사용하고 있어서 데이터를 `mutative` code를 작성해도 state를 업데이트 할 수 있다.
  ```js
  const reducer = createReducer([], {
    [addToDo]: (state, action) => {
      state.push({ text: action.payload, id: Date.now() });
    },
    [deleteToDo]: (state, action) =>
      state.filter((toDo) => toDo.id !== action.payload),
  });
  ```
