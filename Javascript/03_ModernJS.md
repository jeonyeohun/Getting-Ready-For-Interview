## 모던 자바스크립트 



## ES6(ES2015) 주요 업데이트 

- 디폴트 파라미터 : 파라미터의 기본값을 정의할 수 있습니다. 
- 템플릿 리터럴 : 문자열을 표현할 때 변수 등을 + 연산으로 붙이지 않고 표현할 수 있습니다. 
- 화살표 함수 : 화살표 함수를 통해 익명함수를 쉽게 생성할 수 있습니다.
- 프로미스 객체 : 프로미스를 통해 비동기 처리를 쉽게 할 수 있습니다.
- let, const : 스코프 처리가 까다로웠던 var 를 대체하기 위해 블록 스코프를 지원하는 let과 const 가 추가되었습니다.
- Class : 클래스를 만들 수 있는 문법이 추가되었습니다.

## ES7(ES2016) 주요 업데이트

- includes : 배열에 요소 존재여부를 확인할 수 있는 includes 함수가 추가되었습니다.

## ES8(ES2017) 주요 업데이트

- asyc / await : 비동기 처리를 간단하게 할 수 있는 async/await 문법이 추가되었습니다.
- entries / values : 객체의 key-value 를 배열로 반환받는 entries 함수와 객체의 value를 배열로 반환받는 values 함수가 추가되었습니다.

## ES9(ES2018) 주요 업데이트

- rest / spread : 객체와 배열을 복사할 수 있는 rest 와 배열을 분해해 요소들을 꺼낼 수 있는 spread 연산이 추가되었습니다.

## ES11(ES2020) 주요 업데이트

- Optional Chaining : 객체의 참조가능한 값이 있을 때만 참조할 수 있는 문법이 추가되었습니다.

~~~javascript
const person = {
	name: 'hun',
	age: {
		korean : 27,
		american: 25,
	},
	nationality: korea
}

console.log(person.age.korean);
// 결과
// 27

const person = {
	name: 'hun',
	nationality: korea
}

console.log(person.age.korean);
// 결과
// Error 

const person = {
  name: "hun",
  nationality: "korea"
};

console.log(person?.age?.korean);
// 결과
// undefined 
~~~

- null 병합 연산자 : 왼쪽 피연산자가 null 이나 define일 경우에만 오른쪽 피연산자를 반환하고 그 외에는 왼쪽 피연산자를 그대로 반환하는 문법이 추가되었습니다. 0이나 빈 문자열 같은 falsy 값 때문에 발생하는 오류를 최소화할 수 있습니다.

~~~
const foo = null ?? 'default'
console.log(foo)
// 결과
// 'default'
~~~

