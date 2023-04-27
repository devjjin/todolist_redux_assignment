### [과제] 숙련주차 과제 답
- 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음. 
    ```
    // Form.jsx: 액션을 dispatch해서 전달해줘야하는데 누락되어있음
    추가 : const dispatch = useDispatch(); 

    // Form.jsx
    초기화만하고 action 을 dispatch(addTodo)를 하고 있지 않았음
    새로운 todo 객체 만들어서  dispatch에 addTodo액션에 추가해준다.

    추가: 
    const newTodo = {
        id: nextId(),
        title: todo.title,
        body: todo.body,
        isDone: false,
      };
      dispatch(addTodo(newTodo));
    ```
- 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.      
  ```
    // todos.js 
    리듀서의 ADD_TODO액션에서 기존의 todos배열을 복사하지 않아 계속 새로운 값으로 변경됨
    기존의 배열을 복사해 불변성을 유지시켜줘야한다.
    ... todos로 기존 배열 복사후, 추가된 action.payload를 같이 담아준다.
    이전:         // todos: [action.payload],
    수정:  todos: [...state.todos, action.payload],
  ```
- 삭제 기능이 동작하지 않음. 
  ```
   // todos.js
       리듀서에 DELETE_TODO 액션 추가해서
      해당되는 todo.id와 일치하지 않는 것만 새로운 배열로 하도록 switch문에 추가
       추가: case DELETE_TODO:
            return{
              ...state,
              todos: state.todos.filter((todo) => todo.id !== action.payload),
            }
  ```
- 상세 페이지에 진입 하였을 때 데이터가 업데이트 되지 않음.
```
   // 1) Detail.jsx
    detail페이지에 렌더링 될때 dispatch를 하여 해당 id값을 가져오는 액션부분이 없어 해당 todo를 가져오지 못해 
    아래와 같이 getTodoById 함수를 호출해서 dispatch로 전달해줬다.
    
    추가: 
    - useEffect로 dispatch될때 getToById action 추가
     useEffect(() => {
        dispatch(getTodoByID(id));
      },[dispatch])
```
- 완료된 카드의 상세 페이지에 진입 하였을 때 올바른 데이터를 불러오지 못함. 
1) List.jsx
  ```
      리스트를 map으로 돌릴때 link에전달되는 값이  index가 아닌 todo.id를 넘겨주어야
      해당되는 id의 todo의 내용을 가져올수 있고 해당 페이지로 라우팅됨

       - 이전 : {todos.map((todo, index) => {
      - 수정;  {todos.map((todo) => {

        - 이전: <StLink to={`/${index}`} key={todo.id}>
      - 수정:   <StLink to={`/${todo.id}`} key={todo.id}>
                에서 index를 todo.id로 바꿔줌
  ```
1) List.jsx
취소버튼 클릭시, working과 같이 todo.id 를 매개변수로 같이 전달해줘야 해당 todo의 id를 찾아 isDone을 변경할지 알수 있따. 
- 이전:  onClick={onToggleStatusTodo}
- 수정:  onClick={() => onToggleStatusTodo(todo.id)}
