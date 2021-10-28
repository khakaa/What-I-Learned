# redux-tookit

- redux�� ����ϴٺ��� boilerplate �ڵ��� �ϴ� ����ؼ� �ݺ��ϴ� �ڵ带 �ۼ��ϰ� �ȴ�. �̰��� �ذ��ϱ� ���� redux�� ������ ���� redux-tookit�̴�.

## 1. ��Ű�� ��ġ

```zsh
yarn add @reduxjs/tookit
```

---

## 2. createAction

1. redux ����� ��

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

2. redux-tookit ����� ��

- Defining Action Creators with `createAction` : action type�� payload�� ������ش�.

```js

const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

console.log(addToDo, deleteToDo); // f ADD, f DELETE (function)
console.log(addToDo.type, deleteToDo.type); // ADD, DELETE (string)
console.log(addToDo.toString, deleteToDo.toString); // ADD, DELETE (string) -> action type�� ��ȯ�ϵ��� override �Ǿ��ִ�.
console.log(addToDo(), deleteToDo());
// createAction���� ���� �׼��� ȣ���ϸ� �ڵ����� type�� payload�� �������ش�.
// {type : "ADD", payload : undefined}, {type : "DELETE", payload : undefined}


// action creator ��ü�� action type���� �����Ѵ�.
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

1. �׳� reducer

- initialState�� action�� ���� ���ο� state�� ������Ű��, �� ���� store�� �����Ѵ�.
- `switch statements`�� `immutable` updates�� ����ϴ� �Ϲ����� reducer
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

- ù ��° ���ڷδ� initialState,
- �� ��° ���ڷδ� Ư�� action type�� ó���ϴ� reducer case�� ��ü�� ������ �� ����.
  - `createAction`���� ������� action creators�� `computed property syntax`�� ����ϸ� ��ü�� key ���� `action type`���� ���� ����� �� �ִ�.
    ```js
    // Computed property
      [keyName] : (state, action) => { }
    ```
- ���� 1) �Һ��� ������ �ڵ����� ���ش�.
  - ���������� immer ���̺귯���� ����ϰ� �־ `mutative` code�� �ۼ��ص� state�� ������Ʈ �� �� �ִ�. ��, ���༭ ������ �迭�� push �޼ҵ带 ����Ͽ� ���� �迭�� ���� ��ȯ�ϴ� �ڵ带 �ۼ��� �� �ִ�.
  - ������ reducer�� ��� ���� ��ȭ�� ������ `���繰�� ��ȯ�ϴ� ��ü ����`�� �޴´�.
- ���� 2) switch������ case�� ������ �ʰ� reducer�� �ۼ��� �� �ִ�.
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

- �ۼ��� reducer �Լ��� �̸��� ������� action type, action creator�� �ڵ����� ������ش�.

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
