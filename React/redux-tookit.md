# redux-tookit

- redux�� ����ϴٺ��� boilerplate �ڵ��� �ϴ� ����ؼ� �ݺ��ϴ� �ڵ带 �ۼ��ϰ� �ȴ�. �̰��� �ذ��ϱ� ���� redux�� ������ ���� redux-tookit�̴�.

## 1. ��Ű�� ��ġ

```zsh
yarn add @reduxjs/tookit
```

---

## 2. createAction

- Defining Action Creators with `createAction`

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

  ```js

  const addToDo = createAction("ADD");
  const deleteToDo = createAction("DELETE");

  console.log(addToDo, deleteToDo); // f ADD, f DELETE (function)
  console.log(addToDo.type, deleteToDo.type); // ADD, DELETE (string)
  console.log(addToDo(), deleteToDo());
  // {type : "ADD", payload : undefined}, {type : "DELETE", payload : undefined}


  // action creator ��ü�� action type���� �����Ѵ�.
  const reducer = (state = [], action) => {
    switch (action.type) {
    // switch�� ���� createAction���� ���� action type�� ���ٷ��� �ڿ� .type �̳� .toString()���� ���ڿ��� ��ȯ������� �Ѵ�.
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

2. creatorReducer

- ù ��° ���ڷδ� initialState,
- �� ��° ���ڷδ� Ư�� action type�� ó���ϴ� reducer case�� ��ü�� ������ �� ����.
  - `createAction`���� ������� action creators�� `computed property syntax`�� ����ϸ� ��ü�� key ���� ���������� ����� �� �ִ�.
- ���������� immer ���̺귯���� ����ϰ� �־ �����͸� `mutative` code�� �ۼ��ص� state�� ������Ʈ �� �� �ִ�.
  ```js
  const reducer = createReducer([], {
    [addToDo]: (state, action) => {
      state.push({ text: action.payload, id: Date.now() });
    },
    [deleteToDo]: (state, action) =>
      state.filter((toDo) => toDo.id !== action.payload),
  });
  ```
