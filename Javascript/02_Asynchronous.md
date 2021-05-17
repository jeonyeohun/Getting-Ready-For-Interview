# 비동기 처리



## 동기처리와 비동기처리의 차이를 말해보세요.

- 동기처리는 한 요청 작업이 끝나야 다음 작업을 이어서 할 수 있는 방식이고 비동기처리는 한 요청작업이 진행되는 중에 다음작업을 바로 이어서 하는 방식입니다. 자바스크립트는 비동기 처리를 특성으로 가집니다.



## Callback 함수에 대해서 설명해보세요.

- 비동기 처리를 위해 함수호출의 파라미터로 함수를 삽입하는 것을 의미합니다. 호출된 함수가 자신의 로직을 모두 종료하고 콜백함수를 실행하면 해당함수를 호출했던 함수가 미리 지정해둔 콜백함수를 실행합니다.

~~~javascript
function callback (param){
  console.log(param, 'is done!');
}

function callee(param, callback){
  console.log(param);
  callback();
}
function caller(param){
  callee(param, callback);
}

caller('hello');

// 결과:
// hello 
// hello is done! 
~~~



## Promise 에 대해서 설명해보세요.

- 프로미스는 비동기처리를 위한 객체입니다. 
- 프로미스는 생성자의 인자로 콜백 함수를 받고 이 함수는 resolve 와 reject 를 인자로 가집니다. 
- resolve 는 프로미스의 콜백 함수가 성공적으로 완료됐을 때 실행되며 `then` 을 통해서 resolve의 결과 데이터는 받을 수 있습니다.
- reject 는 함수가 실패했을 때 실행되는 로직입니다. 프로미스 함수에 catch 를 사용해 에러 처리 로직을 지정할 수 있습니다.



## Async & Await 에 대해서 설명해보세요.

- async/await 은 비동기 처리를 위해 ES6에서 새롭게 추가된 문법입니다.
- 함수의 선언 앞에 `async` 키워드를 붙이고 Promise 객체를 반환하는 함수의 호출 앞에 `await` 키워드를 붙입니다.
- Promise 보다 간결하고 가독성이 좋은 코드를 작성할 수 있습니다.