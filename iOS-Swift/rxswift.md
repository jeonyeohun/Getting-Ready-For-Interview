## 1. Observable이 무엇인지 설명해주세요.

- Observable은 비동기적으로 이벤트를 생성해서 방출하는 객체입니다. 옵저버블은 next, error, complete 세 개의 이벤트를 방출할 수 있고, 이중 error와 complete이 방출되면 시퀀스가 종료됩니다.

</br>

## 2. Observable은 언제 이벤트를 방출하나요?

- subsribe가 방출하기 전까지는 이벤트를 방출하지 않고 subscribe가 된 시점에서 방출을 시작합니다.

</br>

## 3. empty는?

- empty 옵저버블은 0개의 값을 가지고 있는 옵저버블로 빈 값이 들어있는 next를 한 번 호출하고 곧바로 complete을 호출합니다.

</br>

## 4. never?

- never는 새로운 이벤트를 방출하지도 않고, complete을 방출하지도 않는 옵저버블입니다.

</br>

## 5. dispose와 disposeBag의 차이

- dispose는 옵저버블의 시퀀스를 종료하는 메서드이고, disposeBag은 옵저버블들을 구독하면 반환되는 disposable을 모아두는 객체입니다. disposeBag이 deinit 되면, 내부에 있는 disposable들의 dispose 메서드가 호출됩니다.

</br>

## 6. Traits가 무엇인지

- traits는 옵저버블보다 좀 더 좁은 범위를 가집니다. 
- single, maybe, completable 세 종류가 있습니다. 
- single은 success 와 error를 방출합니다. single은 1회성 이벤트이기 때문에 success나 error가 한번 방출되면 곧바로 completed 됩니다. 데이터 로딩, 네트워킹 등에서 사용할 수 있습니다.
- completable은 completed 와 error만 방출합니다. 파일 쓰기 처럼 연산이 완료되었는지 확인만 하는 용도로 사용할 수 있습니다.
- maybe는 success, completed, error를 모두 방출합니다.

</br>

## 7. Observable과 Subject의 차이

- Observable은 사전에 정의된 이벤트들만 방출하는 반면에 Subject는 next를 통해서 옵저버블이 정의된 이후에도 값을 추가해 이벤트를 방출시킬 수 있습니다. 또한 Subject는 이벤트를 자신을 구독하고 있는 모든 시퀀스로 전달해 멀티캐스팅이 가능합니다.

<br/>

## 8. 멀티캐스팅이 가능하도록 하기 위해서 내부적으로 어떻게 동작하는지 

- subscribe가 호출되면 내부에서 \_observers 프로퍼티에 옵저버를 추가하고 이걸 subscription으로 만들어서 반환합니다. 그래서 새로운 subscribe 발생할 때마다 subject 내부에는 구독자들이 등록되고, next를 통해 이벤트를 방출했을 때 모든 옵저버들의 on을 호출하면서 이벤트를 받게합니다.

<br/>

## 9. 알고있는 Subject 설명

- PublishSubject, BehaviorSubject, ReplaySubject 가 있습니다.
- PublishSubject는 구독이 시작된 이후에 방출된 이벤트를 구독자에게 전달합니다.
- BehaviorSubject는 구독이 시작되기 전 방출되었던 가장 최근 값을 새로운 구독자에게 방출합니다. 첫 구독자라면 지정된 기본값을 방출합니다. 
- ReplaySubject는 버퍼크기를 정해두고 새로운 구독이 시작되면 버퍼 크기만큼의 최근에 방출되었던 값을 모아두었다가 새로운 구독자에게 방출합니다. 

<br/>

## 10. Variables는 모르는지?

- Variables는 BehaviorSubject는 래핑하고 서브젝트의 현재 값을 상태로 가지는 객체입니다. 
- Variable은 error나 completed를 추가할 수 없고 .value에 값을 대입해 새로운 값을 방출하게 할 수 있습니다.
- 어떤 상태를 관찰하고 가지고 있어야 할 때 유용하게 사용할 수 있습니다. 예를 들면 로그인 세션 정보를 가지고 있다가 로그아웃이 될 때 상태를 업데이트해 앱 전역에 이벤트를 전달할 수 있습니다.

<br/>

## 11. 그럼 옵저버블은 멀티캐스트가 아닌가요? 왜 그렇죠?

- 옵저버블은 subscribe가 될 때마다 create 클로저를 생성해서 새로운 disposable을 반환합니다.

<br/>

## 12. Observable은 어떤 타입이나 프로토콜을 채택하는지 말해주고 설명해주세요.

- observable의 상위 프로토콜은 ObservableType이고 subscribe에 대한 인터페이스를 가지고 있습니다. 그리고 ObservableType은 ObservableConvertibleType 프로토콜을 채택하고 있고, 이 프로토콜은 asObservable에 대한 인터페이스를 가지고 있습니다.

<br/>

## 13. DisposeBag이 필요한 이유는?

- DisposeBag이 deinit 되면 disposeBag이 가지고 있던 모든 disposable들에 대해서 dispose 메서드를 순차적으로 호출합니다. 이 과정에서 비동기 처리가 취소처럼 시퀀스를 중단하면서 해야할 작업들을 각 disposable에 대해서 실행할 수 있습니다.

<br/>

## 14. Observable을 커스텀하게 만드는 과정을 설명해주세요.

- create 메서드에 방출할 이벤트들을 정의하는 클로저를 인자로 전달할 수 있습니다. 해당 클로저는 Observer 타입을 파라미터로 받고, 이 observer에 next, completed, error 이벤트를 방출시킬 수 있습니다. 그리고 마지막으로는 시퀀스를 종료시킬 수 있도록 disposables.create 메서드를 호출해 최종적으로 반환할 observable이 disposable을 가질 수 있도록합니다.

<br/>

## 15. drive, bind, subscribe의 차이를 말해주세요.

- 세 방식 모두 특정한 observable에 대해 구독을 시작하고 전달받은 이벤트를 어떻게 처리할지 결정할 수 있게하는 클로저를 정의할 수 있게합니다. 
- bind는 error에 대한 이벤트는 받지 않고 next만 처리합니다. 만약 error가 방출되면 런타임에러가 발생합니다. 
- drive는 bind와 같이 next 이벤트만 처리하지만 모든 이벤트가 메인 스레드에서 실행되는 것을 보장합니다. 

<br/>

## 14. HotObservable과 ColdObservable의 차이는?

- HotObservable은 구독이 시작되지 않아도 이벤트를 바로 방출합니다. 반면에 ColdObservable은 구독이 시작된 이후에 이벤트를 방출합니다.

<br/>

## 15. HotObservable 예시는?

- Subject는 새로운 구독자가 생겨도 이벤트를 처음부터 방출하는 것이 아니라 이벤트를 중간부터 전달하기 때문에 HotObservable이라고 할 수 있습니다. 

<br/>

## 16. subscribe의 동작과정

- subscribe는 내부적으로 subscribe와 함께 전달된 클로저를 가지는 AnonymousObserver를 생성합니다.
- 그리고 Disposable을 생성할 때 현재 자기자신을 subscribe하면서 인자로 observer를 전달하여 생성되는 Disposable을 반환합니다. 

<br/>

## 17. rxswift의 코드를 보면 observer에 대한 코드가 없는데 observer는 어디서 만들어진 것인지?

- subscribe 내에서 observer를 생성하고 있습니다.

<br/>
