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

## 9. 알고있는 Subject 설명

- PublishSubject, BehaviorSubject, ReplaySubject 가 있습니다.
- PublishSubject는 구독이 시작된 이후에 방출된 이벤트를 구독자에게 전달합니다.
- BehaviorSubject는 구독이 시작되기 전 방출되었던 가장 최근 값을 새로운 구독자에게 방출합니다. 첫 구독자라면 지정된 기본값을 방출합니다. 
- ReplaySubject는 버퍼크기를 정해두고 새로운 구독이 시작되면 버퍼 크기만큼의 최근에 방출되었던 값을 모아두었다가 새로운 구독자에게 방출합니다. 

## 10. Observable은 어떤 타입이나 프로토콜을 채택하는지 말해주고 설명해주세요.

## 11. DisposeBag이 필요한 이유는?

## 12. Observable을 커스텀하게 만드는 과정을 설명해주세요.

## 13. drive, bind, subscribe의 차이를 말해주세요.

## 14. HotObservable과 ColdObservable의 차이는?

## 15. subscribe의 동작과정

## 16. rxswift의 코드를 보면 observer에 대한 코드가 없는데 observer는 어디서 만들어진 것인지?

## 17. PublishSubject, BehaviorSubject, ReplaySubject의 차이를 설명해주세요.

## 18. Observable을 직접 구현한다면 어떻게 만들 것인지?
