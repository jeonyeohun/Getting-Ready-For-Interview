# 스위프트 100문 100답을 목표로!

## 1. 스위프트에서 Extension은 어떻게 사용되나요? (What are Extensions used for in Swift?)

- Extension은 클래스, 구조체, 열거형 타입에 새로운 `메서드, 프로퍼티, 생성자`를 추가적으로 정의해 사용하기 위해 사용됩니다.
- 이때 저장 프로퍼티는 extension에 정의할 수 없고, `연산 프로퍼티`만 정의할 수 있습니다.
- 소멸자(deinitializer)는 추가할 수 없고 생성자는 `convenience init` 만 정의할 수 있습니다.
- 구조체의 경우에는 기존 구조체에서는 생성자를 직접 구현하면 `memberwise initializer`(기본 생성자)가 사라지지만 구조체의 Extension에 생성자를 정의하면 `memberwise initializer`가 사라지지 않습니다.
- `where`을 사용하면 특정한 조건을 가진 타입에 대해서만 Extension을 적용할 수 있습니다.

  ```swift
  // https://stackoverflow.com/questions/30746190/swift-where-array-extensions
  // Idable 프로토콜을 채택하는 타입의 배열만 이 Extension을 사용가능
  extension Array where Element : Idable {
      func filterWithId(id : String) -> [Element] {
          return self.filter { (item) -> Bool in
              return item.id == id
          }
      }
  }
  ```

</br>

## 2. Swift의 upcasting과 downcasting의 차이에 대해서 설명해보세요. (What is the difference between Upcast and Downcast in Swift?)

- 서로 상속 관계에 있는 클래스에서 자식 클래스를 부모 클래스로 타입캐스팅 하는 것을 `업 캐스팅`이라고 하고 `as`를 사용해서 업 캐스팅할 수 있습니다. 컴파일 타임에 업캐스팅이 가능한지 여부가 판별되기 때문에 컴파일이 되면 항상 성공합니다.

  ```swift
    class Student {
        let name: String

        init(name: String) {
            self.name = name
        }
    }

    class HighSchoolStudent: Student {
        let gpa: Double

        init(name: String, gpa: Double) {
            self.gpa = gpa
            super.init(name: name)
        }
    }

    let hun = HighSchoolStudent(name: "hun", gpa: 4.5)
    let updacasted = hun as Student

    print(hun.name, hun.gpa)
    print(updacasted.name, updacasted.gpa) // compile error
  ```

- 다운캐스팅도 as 를 사용하지만 다운 캐스팅은 실패할 수도 있기 때문에 `as?`나 `as!`를 사용합니다.
- `as!`는 런타임에 타입 캐스팅을 하고 만약 타입 캐스팅에 실패하면 `런타임 에러`를 발생시킵니다.
- `as?`는 런타임에 타입 캐스팅을 하고 만약 타입 캐스팅에 실패하면 `nil`을 반환합니다.

  ```swift
  let donwcastedHun = hun as? HighSchoolStudent
  print(donwcastedHun) // nil

  let forcelyDowncCastedHun = hun as! HighSchoolStudent // runtime error
  print(forcelyDowncCastedHun)
  ```

</br>

## 3. == 연산자와 === 연산자는 어떻게 다른가요? (What’s the difference between == and ===?)

- `==` 연산자는 값을 비교하는데 사용되고, `===` 연산자는 참조값을 비교하는데 사용됩니다.

</br>

## 4. Dispatch Queue의 Serial Queue에 대해서 설명해보세요. (What is a Serial Queue?)

- 직렬 큐(serial queue)는 작업을 한 번에 하나씩 처리하는 작업 큐입니다. 직렬 큐는 스레드에 먼저 할당한 작업이 완전히 끝나야 큐에서 대기 중인 다음 작업을 스레드에 새로 할당합니다.

</br>

## 5. let과 var의 차이는 무엇인가요? (What is the difference between let and var in Swift?)

- `let`은 한번 할당된 값을 변경할 수 없는 상수를 선언하기 위한 명령어이고, `var`는 값을 변경할 수 있는 변수를 선언하기 위한 명령어입니다.
- 사실 Obejctive-C 관점에서 `let`은 주소에 대한 포인터를 바꿀 수 없다는 의미입니다. 이 때문에 클래스 인스턴스의 var로 선언된 변수의 값을 바꾸는 것은 가능합니다.

  ```swift
  let hun = Student(name: "hun")
  hun.name = "hunihun965"

  print(hun.name) // hunihun956
  ```

</br>

## 6. function과 method의 차이를 말해보세요. (What are the differences between functions and methods in Swift?)

- function은 재사용 가능한 코드 블록을 의미하고, method는 클래스, 구조체, 열거형에 포함되는 function을 의미합니다.

</br>

## 7. 커스텀 객체의 배열이 있을 때, 프로퍼티를 기준으로 배열을 어떻게 정렬할 수 있을까요? (How to sort array of custom objects by property value in Swift?)

- sorted 메서드에 직접 소팅 클로저를 지정해줄 수 있습니다.

  ```swift
  struct Node {
      let id: Int
  }

  let array = [
      Node(id: 5),
      Node(id: 4),
      Node(id: 3),
      Node(id: 2),
      Node(id: 1)
  ]

  let sorted = array.sorted(by: { $0.id < $1.id })
  print(sorted)
  // [PS.Node(id: 1), PS.Node(id: 2), PS.Node(id: 3), PS.Node(id: 4), PS.Node(id: 5)]
  ```

</br>

## 8. mutaing 키워드의 의미를 설명해보세요. (What does the Swift mutating keyword mean?)

- 스위프트에서 값 타입 프로퍼티들을 `인스턴스 메서드에 의해 수정될 수 없습니다`.
- mutaing 키워드를 메서드 앞에 붙이면 구조체나 열거형 인스턴스에서 프로퍼티를 수정할 수 있게 됩니다.
- mutating 키워드가 붙은 메서드를 실행하면 스위프트는 `새로운 구조체를 생성`해 변경된 프로퍼티의 값을 할당하고 반환해 현재 구조체를 대체합니다. 구조체의 불변성을 지키기 위해 이런 방법을 사용합니다.

</br>

## 9. 프로토콜과 클래스의 차이를 설명해보세요. (What’s the difference between a protocol and a class in Swift?)

- 클래스는 인스턴스 메서드의 실제 구현체를 가지고 있지만 프로토콜은 메서드의 `인터페이스`만 가지고 있습니다. 프로토콜이 구현체를 가지게 하려면 프로토콜의 `Extension`을 만들어 구현체를 작성할 수 있습니다.

</br>

## 10. Enum에서 raw value와 associated value에 대해 설명해보세요. (In Swift enumerations, what’s the difference between raw values and associated values?)

- r`aw value`는 원시값으로 열거형의 모든 case들이 동일한 타입을 가지고 하나의 값만 가질 수 있습니다.
- 반면에 `associated value`는 튜플을 통해 각 case들이 다른 타입을 가지게 할 수도 있고, named tuple로 이름을 붙일 수도 있으며, 여러개의 값을 가지게 하는 것도 가능합니다.

  ```swift
  // associated values
  enum Fruits {
  case apple(origin: String, cost: Double)
  case grape(origin: String, cost: Double, size: Double)
  case orange(color: String)
  }

  let apple: Fruits = .apple(origin: "Korea", cost: 1000)
  let grape: Fruits = .grape(origin: "Korea", cost: 100, size: 10)
  let orange: Fruits = .orange(color: "Orange")

  // raw values
  enum Numbers: Int {
  case one = 1
  case two, three, four
  }

  print(Numbers.one.rawValue)   // 1
  print(Numbers.two.rawValue)   // 2
  print(Numbers.three.rawValue) // 3
  ```

</br>

## 11. inout은 언제 사용하면 좋을까요? (What is a good use case for an inout parameter?)

- `inout` 파라미터를 사용하면 값 타입 변수가 저장된 주소의 값을 함수 안과 밖에서 동일하게 사용하게 됩니다. 따라서 함수가 입력과 동일한 출력을 제공하고, 함수 내에서 적용된 변경사항이 함수 외부에서도 동일하게 적용되어야할 때 사용할 수 있습니다. Swap을 직접 구현하고자 한다면 inout이 좋은 선택이 될 수 있습니다.

</br>

## 12. 연산 프로퍼티와 클로저를 가지는 저장 프로퍼티의 차이를 설명해보세요. (What is the difference between a computed property and a property set to a closure?)

- 클로저를 가지는 저장 프로퍼티는 프로퍼티의 생성시점에 클로저를 생성하고 사용합니다. 또한 var 키워드로 생성되어 있다면 다른 클로저를 할당해주는 것도 가능합니다.
- 연산 프로퍼티는 프로퍼티를 `참조할 때마다` 클로저를 생성하고 실행합니다.

</br>

## 13. as? 와 as! 차이를 설명해보세요. (What is difference between as?, as! and as in Swift?)

- as? 와 as! 모두 런타임에 `다운 캐스팅`을 위해 사용되지만 `as?` 는 캐스팅에 실패했을 때 nil을 반환하고 `as!` 는 런타임 에러를 발생시킵니다.

</br>

## 14. 메서드 안에서 언제 self를 사용해야할까요? (When would you use self in a method?)

- 파라미터의 이름이 인스턴스의 프로퍼티 이름과 겹칠 경우에 `인스턴스의 프로퍼티임을 명시`하기 위해서 self를 사용할 수 있습니다.

</br>

## 15. Class와 Struct의 공통점과 차이점을 설명해보세요. (What Classes and Structs have in common in Swift and what are their differences?)

- Class 와 Struct 모두 프로퍼티를 정의해 데이터를 저장하고 `인스턴스를 만들어 객체의 형태`로 사용할 수 있습니다.
- Class 와 Struct 모두 `프로토콜을 채택`할 수 있습니다.
- Class 는 `참조 타입`이지만 Struct는 `값 타입`입니다.
- Class 는 참조 타입이기 때문에 최적화를 사용하지 않지만 구조체는 값 타입의 복사를 최적화하기 위해서 `Copy-On-Write`를 사용합니다. 실제로 변경이 일어나기 전까지는 구조체를 다른 변수에 할당하더라도 같은 주소의 구조체를 공유하다가 변경이 발생하면 새로운 주소에 인스턴스를 생성해 할당하는 것을 의미합니다.
- Class 는 `힙 영억`에 인스턴스를 저장하고 인스턴스의 주소값을 스택 영역에 저장합니다. 반면에 Struct는 `스택 영역`에 인스턴스가 저장됩니다.
- Class 는 인스턴스끼리의 `상속`이 가능하지만 Struct는 불가능합니다. 대신 `프로토콜`을 통해 상속의 동작을 구현할 수는 있습니다.

</br>

## 16. 강한 참조는 무엇이고 왜 필요한가요? (What’s a strong reference, and why do we need it?)

- 강한 참조는 참조 타입 인스턴스를 변수에 할당하는 것을 의미합니다. 스위프트의 ARC는` 강한 참조에 참조 카운트를 증가`시키고, 강한 참조가 해제되면 참조 카운트를 감소시킵니다. 그리고 참조 카운트가 `0이되면 메모리에서 인스턴스를 해제`합니다. 따라서 강한 참조가 있어야만 스위프트에서 참조 타입의 인스턴스르 메모리에 유지할 수 있습니다.

</br>

## 17. strong, weak, unowned reference는 각각 언제 사용할까요? (When to use strong, weak and unowned references?)

- 메모리에서 인스턴스가 해제되는 것을 막기 위해 강한 참조인 `strong reference`를 사용할 수 있습니다. strong reference는 `참조 카운트를 1 증가`시키기 때문입니다. 기본적으로 참조의 방향이 단방향으로 이루어지면 strong reference는 항상 안전합니다.
- `weak reference`는 `참조 카운트를 증가시키지 않습니다`. weak reference는 항상 var 로 선언되는 옵셔널 타입이 되어야합니다. weak으로 참조하고 있던 인스턴스가 해제될 수 있기 때문입니다. 순환 참조의 가능성이 있는 상황에서 weak을 통해 방지할 수 있습니다.
- `unowned reference`는 weak 과 동일하게 참조 카운트를 증가시키지 않습니다. 그리고 동시에 unowned 로 선언된 변수는 `nil을 가질 수 없다`는 특징이 있습니다. unowned 로 참조하고 있던 인스턴스가 해제되면 unowned 는 nil이 아니라 더 이상 참조할 수 없는 주소를 계속 참조하게 되고 unowned 변수를 참조하려고 하면 런타임 에러가 발생합니다. 따라서 unowned는 `해당 변수가 참조하는 인스턴스보다 먼저 해제되는 것이 확실한 상황에서만 사용`해야 합니다. Optional이 아니기 때문에 캡쳐 리스트나 옵셔널 바인딩을 사용하지 않아도 된다는 장점이 있습니다.

</br>

## 18. Array, Set, Dictionary의 차이점이 뭘까요? (What's the main difference between the Array, Set and Dictionary collection type?)

- Array는 리스트 컬렉션으로 `Random Access`가 가능해 인덱스를 통해 요소에 접근할 수 있습니다.
- Set은 `Hashable 프로토콜을 채택`하는 값을 저장해 중복되지 않은 데이터를 관리하는 콜렉션입니다. 순서가 보장되지 않으며, 교집합, 차집합 등 집합 연산을 메서드로 제공합니다.
- Dictionary 는 `Key-Value` 형태로 데이터를 관리하는 콜렉션입니다. 딕셔너리의 `Key로 사용될 타입은 Hashable 프로토콜을 채택`하고 중복된 키를 허용하지 않으며 순서를 보장하지 않습니다.

</br>

## 19. required 키워드에 대해서 설명해보세요. (What does the required keyword in Swift mean?)

- `required` 키워드가 붙은 클래스의 생성자는 해당 클래스를 상속받는 자식 클래스가 해당 생성자를 반드시 구현하도록 강제합니다.
- required 에는 `override` 키워드의 기능이 포함되어 있기 때문에 override를 생략하고 구현할 수 있습니다.

</br>

## 20. Self와 self의 차이가 뭘까요? (What's the difference between Self vs self?)

- `self`는 현재 인스턴스를 가리킵니다.
- `Self`는 프로토콜에서 사용되면 프로토콜을 채택하는 타입을 의미하고, 클래스, 구조체, 열거형에서 사용되면 실제 선언에 사용된 타입을 의미합니다.

  ```swift
  class Superclass {
  func f() -> Self { return self }
  }
  let x = Superclass()
  print(type(of: x.f()))
  // Prints "Superclass"

  class Subclass: Superclass { }
  let y = Subclass()
  print(type(of: y.f()))
  // Prints "Subclass"

  let z: Superclass = Subclass()
  print(type(of: z.f()))
  // Prints "Subclass"
  ```

  -> MetaType에 대한 공부가 필요할 것 같습니다ㅠ 헷갈리네요..

</br>

## 21. Array보다 Set을 사용하는게 더 좋을 때는 언제일까요? (When to use a set rather than an array in Swift?)

- `순서가 중요하지 않고 데이터를 중복없이 고유하게 관리`할 때 Set을 사용하는 것이 더 좋습니다.
- 특히, Set은 삭제, 삽입, 조회를 모두 O(1)에 할 수 있기 때문에 `순서가 중요하지 않으면서 삭제와 삽입이 빈번할 때`도 Set이 더 좋을 수 있습니다.

</br>

## 22. Trailing Closure에 대해서 설명해보세요. (What is trailing closure syntax?)

- 함수의 인자로 들어갈 클로저를 함수 호출 외부로 분리해서 코드를 작성하는 방법입니다.
- trailing closure의 파라미터 레이블을 생략할 수 있고, 만약 호출하는 함수의 파라미터가 클로저 뿐이라면 `()` 도 생략할 수 있습니다.

  ```swift
  func someFunctionThatTakesAClosure(closure: () -> Void) {
  // function body goes here
  }

  // Here's how you call this function without using a trailing closure:

  someFunctionThatTakesAClosure(closure: {
      // closure's body goes here
  })

  // Here's how you call this function with a trailing closure instead:

  someFunctionThatTakesAClosure() {
      // trailing closure's body goes here
  }
  ```

</br>

## 23. @objc는 언제 사용하나요? (When to use @objc attribute?)

- @objc 는 스위프트의 API를 Objective-C 런타임에 사용할 수 있도록 하기위해 사용합니다.

</br>

## 24. deinit은 언제 사용할까요? (When should I use deinit?)

- `deinit`은 인스턴스가 메모리에서 해제되기 직전에 호출됩니다. 인스턴스를 해제하기 전에 선행되어야 하는 작업이 있다면 deinit에 구현할 수 있습니다.

</br>

## 25. DispatchQueue.main.async 와 DispatchQueue.main.sync 의 차이를 설명해보세요. (Explain the difference between DispatchQueue.main.async and DispatchQueue.main.sync?)

- 두 방법 모두 DispatchQueue에 작업을 등록하고 `Main 스레드에서 작업이 수행되도록 합니다`.
- `DispatchQueue.main.async`는 작업을 등록할 때 async 하게 등록하기 때문에 등록한 `작업이 끝나길 기다리지 않고 등록 후 곧바로 다음 코드를 실행`합니다.
- `DispatchQueue.main.sync`는 작업을 등록할 때 sync 하게 등록하기 때문에 `등록한 작업이 끝날 때 까지 다음 코드로 진행하지 않습니다.` 이때 작업을 등록한 스레드 역시 메인 스레드라면 스레드가 sync에 의해 동작을 멈춘 상태에서 메인스레드에 큐에 등록된 작업이 할당됩니다. 메인 스레드는 큐에 등록했던 작업이 끝나길 기다리고, 동시에 메인 스레드에 할당된 작업은 실행되길 기다리기 때문에 `데드락 상태`에 빠지게 됩니다.

</br>

## 26. Defer에 대해 설명해보세요. (Explain the defer usage in Swift)

- `defer`는 클로저에 정의된 코드가 읽어진 이후에 함수가 끝나기 전 마지막에 실행되도록 합니다.

  ```swift
  func deferTest() {
      print(1)
      defer {
          print("last")
      }
      print(2)
  }

  deferTest()
  // 1
  // 2
  // last
  ```

- defer 코드가 실행되지 않으면 함수가 끝나도 클로저는 실행되지 않습니다.

```swift
func deferTest() {
    print(1)
    return
    defer {
        print("last")
    }
    print(2)
}

deferTest()
// 1
```

- 여러개의 defer가 실행되었다면 읽혀진 역순으로 실행됩니다.

```swift
func deferTest() {
    print(1)
    defer {
        print("last1")
    }
    defer {
        print("last2")
    }
    defer {
        print("last3")
    }
    print(2)
}

deferTest()
// 1
// 2
// last3
// last2
// last1
```

- 중첩된 defer는 바깥쪽 defer 부터 실행됩니다.

```swift
func deferTest() {
    print(1)
    defer {
        print("last1")
        defer {
            print("last2")
            defer {
                print("last3")
            }
        }
    }
    print(2)
}

deferTest()
// 1
// 2
// last1
// last2
// last3
```

</br>

## 27. What is the difference between open and public keywords in Swift?

</br>

## 28. When to use fileprivate access modifier in Swift?

</br>

## 29. What is the difference between fileprivate and private?

</br>

## 30. What is the difference between static func and class func in Swift?

</br>

## 31. What is the difference between functions and closures?

</br>

## 32. Can you rewrite this code using mutating function?

</br>

## 33. Are there any differences between Protocol in Swift vs Interface in Java? Related To: Java

</br>

## 34. Is there a way to create an abstract class in Swift?

</br>

## 35. What’s the difference between Any and AnyObject?

</br>

## 36. Explain when to use different Swift casting operators?

</br>

## 37. When should you use Structs over Classes?

</br>

## 38. When should you use Classes over Structs?

</br>

## 39. Explain the difference between weak and unowned references. Provide an example.

</br>

## 40. When is it safe to use an unowned reference?

</br>

## 41. What is Copy on Write (Cow) in Swift?

</br>

## 42. What’s the difference between a static variable and a class variable?

</br>

## 43. What is the difference between ARC (automatic reference counting) and GC (garbage collection)?

</br>

## 44. What is the autoclosure attribute and when to use it?

</br>

## 45. What is QoS (Quality of Service) in GCD?

</br>

## 46. Explain how that code will behave for different Swift versions?

</br>

## 47. What is the use of Hashable protocol?

</br>

## 48. What’s the difference between init?() and init()?

</br>

## 49. What is the Never return type? When to use it over Void?

</br>

## 50. Do we need to use [weak self] or [unowned self] in this closure?

</br>

## 51. Why can not we just use weak everywhere and forget about unowned?

</br>

## 52. Explain the use case when ARC won't help you to release memory (but GC will)?

</br>

## 53. Explain what is DispatchGroup?

</br>

## 54. What are the benefits of using DispatchWorkItem in Swift? Related To: iOS

</br>

## 55. Explain usage of Concurrent vs Serial Queues with async and sync blocks Related To: iOS

</br>

## 56. What is the difference between @escaping and @nonescaping Closures in Swift?

</br>

## 57. What's the difference between marking a method as @objc vs dynamic, when would you do one vs the other?

## References

https://www.fullstack.cafe/interview-questions/swift
