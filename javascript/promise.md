## Promise

프로미스 객체는 비동기 작업이 맞이할 어떤 시점의 미래의 완료 또는 실패와 그 결과 값을 제공해주는 객체이다. 프로미스 객체를 사용하면 비동기 연산을 마치 동기 연산처럼 값을 반환할 수 있다. 세가지 상태정보를 갖으며, 비동기 처리 시점을 명확하게 표현할 수 있다.

- pending (대기) : 비동기 처리가 아직 수행되지 않은 상태
- fullfilled (이행) : 비동기 처리가 성공적으로 수행된 상태
- rejected (실패) : 비동기 처리가 실패한 상태

프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야한다.

- .then(handleSuccess, handleError) : 비동기 처리가 성공했을 때 호출되는 성공 처리 콜백 함수
  - handleSuccess : 프로미스가 fullfilled 상태일 때 (resolve 함수가 호출된 상태) 호출되며, 이 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
  - handleError : 프로미스가 rejected 상태일 때 (reject 함수가 호출된 상태) 호출되며, 이 콜백 함수는 프로미스의 에러를 인수로 전달받는다.
- .catch() : 프로미스가 rejected 상태일 때만 호출된다. then메서드 두 번째 콜백 함수에서 에러 처리를 하는 것보다 가독성이 좋고 명확해서 catch메서드에서 하는 것을 권장한다고 한다.
- .finally() : 프로미스의 성공, 실패와 관계없이 무조건 한 번 호출. 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다.

## **async/await**

프로미스의 후속 처리 메서드 없이 마치 동기처리 처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

- async 함수 : await 키워드는 반드시 async 함수 내부에서 사용해야한다. async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다. async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
- await 키워드 : 프로미스가 settled (비동기 처리가 수행된 상태) 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리결과를 반환한다. await 키워드는 반드시 프로미스 앞에서 사용해야 한다.
- async/await 에서 에러 처리는 **try ... catch** 문을 사용할 수 있다.
