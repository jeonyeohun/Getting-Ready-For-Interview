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

- `raw value`는 원시값으로 열거형의 모든 case들이 동일한 타입을 가지고 하나의 값만 가질 수 있습니다.
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

## 27. open과 public 키워드의 차이를 설명해보세요. (What is the difference between open and public keywords in Swift?)

- `open`과 `public` 키워드 모두 `외부 모듈에서의 접근까지 허용`합니다.
- open은 `클래스에서만 사용`할 수 있습니다.
- open은 외부 모듈에서 클래스를 상속하는 것과 메소드 오버라이딩이 가능하지만 public은 외부 모듈에서의 클래스 상속과 메소드 오버라이딩을 `제한`합니다.
- 동일한 모듈 내에서는 open과 public 모두 클래스 상속과 메소드 오버라이딩이 가능합니다.

</br>

## 28. fileprivate을 설명하고 언제 사용하면 좋을지 이야기해보세요. (When to use fileprivate access modifier in Swift?)

- fileprivate은 `같은 소스파일 내에서의 접근만 하용`합니다.
- fileprivate인 인스턴스를 할당하는 변수는 `private이거나 fileprivate`으로 접근이 제한되어야 합니다.
- fileprivate인 클래스를 상속할 때도 상속받는 클래스가 `private이거나 fileprivate`이어야 합니다.
- 같은 파일 내부에서만 사용되는 클래스일 때 fileprivate로 제한하면 유용하게 사용할 수 있습니다.

</br>

## 29. fileprivate과 private의 차이를 설명해보세요. (What is the difference between fileprivate and private?)

- fileprivate은 같은 파일 내부에 있다면 접근을 허용했지만 private은 같은 파일에 있어도 private으로 선언한 대상의 `구현부 내부`, 그리고 같은 파일에 있는 `동일한 선언의 Extension` 에서의 접근만 허용합니다.

```swift
private class A {
  public init() {}
  private var data: String
}

extension A { // 같은 파일에 있을 때만 가능
  func updateData(data: String) {
    self.data = data
  }
}

```

</br>

## 30. static func 와 class func의 차이를 설명해보세요. (What is the difference between static func and class func in Swift?)

- static func, class func 모두 `타입 메소드`이기 때문에 인스턴스를 생성하지 않고 타입에 접근해 함수를 호출할 수 있습니다.
- class func는 `오버라이딩`을 허용하지만 static func는 오버라이딩을 허용하지 않습니다.
- 스위프트에서 유일하게 직접적인 상속을 지원하는 객체타입이 클래스라는 것을 생각해보면 class만 오버라이딩을 허용하는 것과 쉽게 연결시킬 수 있습니다!

</br>

## 31. Function과 Closure의 차이를 설명해보세요. (What is the difference between functions and closures?)

- Function과 Closure 모두 실행가능한 코드블록을 의미합니다.
- Function은 func 키워드와 함께 선언되고 `함수의 이름`을 항상 가져야합니다. 반면에 클로저는 이름을 가지지 않습니다.

</br>

## 32. 스위프트에서 추상 클래스를 만드려면 어떻게 해야할까요? (Is there a way to create an abstract class in Swift?)

- 스위프트는 추상 클래스 문법을 지원하지는 않지만 프로토콜을 통해 동일한 동작을 하도록 할 수 있습니다.
- 프로퍼티와 메서드의 원형을 프로토콜에 선언해두고, `Extension`을 통해 프로토콜 메서드의 기본 구현체를 만들어주면 추상클래스와 동일한 개념의 구현을 할 수 있습니다.

</br>

## 33. Any와 AnyPbject의 차이를 설명해보세요. (What’s the difference between Any and AnyObject?)

- Any와 AnyObject 모두 범용타입으로 여러 타입을 한번에 표현할 수 있게 해주지만 AnyObject는 `클래스 타입만` 저장할 수 있다는 제한조건이 추가됩니다.

</br>

## 34. 언제 클래스 대신 구조체를 사용하면 좋을까요? (When should you use Structs over Classes?)

- 스위프트에서는 기본적으로 구조체를 사용하길 권장하고 있습니다.
- 구조체는 불변성을 유지하기 때문에 여러 스레드들이 한 인스턴스를 사용하는 `다중 스레드 환경에서도 안전`하게 사용될 수 있습니다.

</br>

## 35. 언제 구조체 대신 클래스를 선택해야할까요? (When should you use Classes over Structs?)

- 구조체는 값 타입이기 때문에 값이 같은 인스턴스가 매번 복사되어 사용됩니다.
- 따라서 만약 어떤 인스턴스의 참조값의 `고유성을 유지`하고 싶다면 클래스를 사용할 수 있습니다.
- 또한 `Objective-C의 API를 사용할 때`는 클래스가 반드시 필요하기도 합니다. i.e.)NSCache의 value에는 구조체 인스턴스를 사용할 수 없습니다.

</br>

## 36. weak과 unowned 의 차이를 설명하고 예를 들어주세요. (Explain the difference between weak and unowned references. Provide an example.)

- weak은 참조하고 있던 인스턴스가 해제되는 것을 염두하여 항상 Optional한 타입을 가집니다. 만약 weak으로 선언한 변수가 참조하고 있던 인스턴스가 메모리에서 해제되면 해당 변수의 값은 nil로 채워집니다.
- unowned는 참조하고 있는 인스턴스가 `unowned 변수 이전에는 절대 해제되지 않음을 보장하는 상황`에서 사용합니다. Optional이 아닌 타입을 가집니다. 따라서 unowned가 참조하고 있던 인스턴스가 해제된 이후에 unowned 변수를 참조하면 런타임 에러가 발생합니다.

```swift
// https://stackoverflow.com/questions/24011575/what-is-the-difference-between-a-weak-reference-and-an-unowned-reference
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
}

class Apartment {
    let number: Int
    init(number: Int) { self.number = number }
    weak var tenant: Person?
}
// Person ===(strong)==> Apartment
// Person <==(weak)===== Apartment

class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) { self.name = name }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) { self.number = number; self.customer = customer }
}

// Customer ===(strong)==> CreditCard
// Customer <==(unowned)== CreditCard
```

</br>

## 37. unowned는 언제 사용하는 것이 안전할까요? (When is it safe to use an unowned reference?)

- unowned로 선언된 변수가 참조하는 인스턴스가 해당 unowned 변수가 해제된 이후에 해제되는 것이 보장되는 상황에서 사용하는 것이 안전합니다.

```swift
// https://www.advancedswift.com/strong-weak-unowned-in-swift/
extension HomeVC {
    // Calling loadAPI no longer creates a Retain Cycle:
    // 1. Self (HomeVC) has a strong reference to api
    // 2. api has a strong reference to completion
    // 3. completion has a unowned reference to self (HomeVC)
    func loadAPI() {
        api.completion = { [unowned self] (data, error) in
            self.hideLoadingIndicator()
        }
    }
    // HomeVC -> api -> completion === (unonwned) ===> HomeVC
}
```

</br>

## 38. Swift의 Copy-on-Write에 대해서 설명해보세요. (What is Copy on Write (Cow) in Swift?)

- COW는 값 타입의 복사를 `실제로 값이 수정되기 전까지는 발생시키지 않아 메모리 사용을 최적화`하는 방법입니다. 예를 들어서 값타입인 배열을 두 변수에 저장해두면 두 개의 다른 인스턴스가 생성될 것을 기대하지만 실제로는 주소 값을 공유하고 있다가 변수에 변경사항이 생겼을 때 새로운 인스턴스를 할당합니다.
- 이렇게 하면 수정이 일어나지 않는 값 타입 데이터는 복사본을 만들지 않고 사용되기 때문에 메모리 사용을 최적화할 수 있습니다.

```swift
import Foundation

func compareValueTypeInstance(of arr1:[Int], with arr2:[Int]) {
    if arr1 == arr2 {
        print("same instance")
    } else {
        print("different istance")
    }
}

let arr1 = [1, 2, 3, 4]
var arr2 = arr1

compareValueTypeInstance(of: arr1, with: arr2) // "same instance"

arr2.append(5)

compareValueTypeInstance(of: arr1, with: arr2) // "different istance"
```

</br>

## 39. staic 변수와 class 변수에 대해 설명해보세요. (What’s the difference between a static variable and a class variable?)

- static 변수와 class 변수 모두 `타입 프로퍼티`로 클래스, 구조체, 열거형에 모두 사용할 수 있습니다.
- 타입 프로퍼티는 `lazy`하게 동작해서 실제로 불리기 전까지는 메모리에 로드되지 않습니다.
- 타입 프로퍼티는 한 번 불리면 메모리에 로드되고 그 이후로는 `새로 생성되지 않습니다`.
- 타입 프로퍼티는 타입 이름을 통해서만 접근이 가능하고 `초기값을 항상 가져야합니다`.
- `class`로 선언된 타입 프로퍼티는 `오버라이딩이 가능`합니다.
- 반면에 `static`으로 선언된 타입 프로퍼티는 `오버라이딩이 불가능`합니다.

</br>

## 40. ARC와 GC는 어떤 차이점이 있나요? (What is the difference between ARC (automatic reference counting) and GC (garbage collection)?)

- ARC와 GC의 가장 큰 차이점은 런타임에 처리를 하는지, 컴파일 타임에 처리를 하는지에 있습니다. `ARC`는 Retain과 Release를 컴파일러가 `컴파일 타임`에 자동으로 삽입해 Reference Count를 조절합니다.
- 반변에 `GC`는 가비지 콜렉터를 `런타임`에 별도로 실행하면서 메모리의 상태를 감시합니다.
- ARC는 단순히 카운팅을 통해 인스턴스들을 관리하기 때문에 순환참조의 위험이 있습니다.
- GC는 `Mark-and-Sweep` 방식으로 루트노드부터 도달 가능한 인스턴스들을 모두 체크합니다. 따라서 순환참조가 발생하더라도 두 인스턴스를 참조하는 인스턴스가 해제되면 루트로부터 순환참조가 발생한 인스턴스들에 도달할 수 있는 경로가 없기 때문에 두 인스턴스를 모두 해제할 수 있습니다.

</br>

## 41. autoclosure attribute에 대해서 설명해보세요. (What is the autoclosure attribute and when to use it?)

- autoclosure는 클로저가 아닌 코드를 함수의 인자로 받을 때 이 `코드를 클로저로` 만들어주는 키워드입니다.
- autoclosure의 주된 목적은 코드의 `실행을 지연`시키기 위함입니다. 클로저를 인자를 넘기는 것으로도 지연 실행을 만족시킬 수 있지만 autoclosure를 사용하면 호출하는 측 코드의 가독성이 좋아집니다.

- 클로저 사용 X

```swift
// https://www.avanderlee.com/swift/autoclosure/

 struct Person: CustomStringConvertible {
   let name: String

   var description: String {
       print("Asking for Person description.")
       return "Person name is \(name)"
   }
}

let isDebuggingEnabled: Bool = false

func debugLog(_ message: String) {
    /// You could replace this in projects with #if DEBUG
    if isDebuggingEnabled {
        print("[DEBUG] \(message)")
    }
}

let person = Person(name: "Bernie")
debugLog(person.description) // 디버깅 모드가 아니어도 person.description이 실행됨
```

- 클로저 사용

```swift
 let isDebuggingEnabled: Bool = false

func debugLog(_ message: () -> String) {
    /// You could replace this in projects with #if DEBUG
    if isDebuggingEnabled {
        print("[DEBUG] \(message())")
    }
}

let person = Person(name: "Bernie")
debugLog({ person.description }) // 클로저가 넘어가기 때문에 description 참조가 실행되지 않음
```

- autoclosure 사용

```swift
 let isDebuggingEnabled: Bool = false

func debugLog(_ message: @autoclosure () -> String) {
    /// You could replace this in projects with #if DEBUG
    if isDebuggingEnabled {
        print("[DEBUG] \(message())")
    }
}

let person = Person(name: "Bernie")
debugLog(person.description) // 클로저로 래핑되면서 들어가기 때문에 description 참조가 실행되지 않음
```

</br>

## 42. GCD의 QoS에 대해서 설명해보세요. (What is QoS (Quality of Service) in GCD?)

- Qos 는 DispatchQueue에 등록하는 `작업의 우선순위`를 결정할 수 있게 합니다.
- 우선순위가 높은 작업은 우선순위가 낮은 작업보다 먼저 실행되지만 앱의 리소스를 많이 사용합니다.
- qos 수준은 다음으로 정의됩니다.

- `userInterative`: 가장 우선순위가 높습니다. UI작업 등 사용자에게 즉각적인 반응을 해야하는 작업에 사용합니다.
- `userInitiated`: 문서를 열람하거나 인터페이스에 제스쳐를 취하는 등 사용자와의 상호작용이 시작되었을 때 곧바로 결과를 반환해야 하는 작업에 사용합니다.
- `default`: 기본값입니다. 일반적인 작업에 사용합니다.
- `utility`: 데이터를 다룬로드 하는 등 결과를 만들기 위해 시간이 걸리는 작업에 사용합니다. 프로그래스 바나 액티비티 인디케이터와 함께 사용합니다.
- `background`: 사용자가 인지하지 못하는 영역에서 에너지와 리소스를 효율적으로 사용하기 위해 사용합니다.

</br>

## 43. Hashable 프로토콜에 대해서 설명해보세요. (What is the use of Hashable protocol?)

- Hashable 프로토콜을 채택하는 타입은 모두 값을 `정수인 해시값`으로 표현할 수 있습니다.
- 스위프트의 기본 타입 중 `문자열, 정수, 실수, 불리언, 그리고 Set 콜렉션`이 Hashable 프로토콜을 채택하고 있습니다.
- Hashable 프로토콜을 채택하는 커스텀 타입의 `저장 프로퍼티가 모두` Hashable 프로토콜을 채택하고 있다면, 별다른 구현없이 Hashable 프로토콜을 채택하는 것 만으로 Hashable한 동작을 제공합니다.
- 그렇지 않다면 `==` 메서드를 만들고 hash(into:) 구현해서 해시값을 생성하는데 필요한 프로퍼티를 지정해주어야합니다.

```swift
// https://developer.apple.com/documentation/swift/hashable
struct GridPoint {
  var x: Int
  var y: Int
}

extension GridPoint: Hashable {
  static func == (lhs: GridPoint, rhs: GridPoint) -> Bool {
      return lhs.x == rhs.x && lhs.y == rhs.y
  }

  func hash(into hasher: inout Hasher) {
      hasher.combine(x)
      hasher.combine(y)
  }
}
```

</br>

## 44. 열거형도 Hashable을 채택했을 때 자동으로 Hashable하게 만들 수 있나요?

- 열거형은 `associated value`가 없는 경우에는 Hashable 프로토콜만 채택해도 Hashable하게 사용할 수 있습니다.
- 하지만 associated value가 있는 경우에는 `hash` 메서드를 구현해주어야 합니다.

## 45. init?()과 init()은 어떤 차이가 있나요? (What’s the difference between init?() and init()?)

- init?()은 `실패가능한 생성자`로 생성자의 코드를 실행하다가 문제가 생겼을 때 nil을 반환하도록 할 수 있습니다.

```swift
class A {
  let a: String

  init?(a: String) {
      guard !a.isEmpty else { return nil }
      self.a = a
  }
}

print(A(a: ""))
print(A(a: "abc")?.a)
```

</br>

## 46. Never 반환 타입에 대해 설명해보세요. (What is the Never return type? When to use it over Void?)

- Never 타입은 `비정상적인 종료`에 사용되는 반환 타입이며 값을 지니지는 않습니다.
- 클로저, 함수에 에러를 throw하는 것으로 잡을 수 없는 심각한 오류가 있음을 알릴 때 사용할 수 있습니다.

```swift
func crashAndBurn() -> Never {
    fatalError("Something very, very bad happened")
}
```

</br>

## 47. weak만 사용하지 않고 unowned도 사용하는 이유가 무엇을까요? (Why can not we just use weak everywhere and forget about unowned?)

- unowned를 사용하면 `옵셔널`을 신경쓰지 않아도 된다는 장점이 있습니다.
- unowned를 사용하면 참조하고 있는 인스턴스가 unowned 프로퍼티 이전에 항상 메모리에서 해제되는 것을 `명시적으로 표현`할 수 있습니다.

</br>

## 48. ARC가 메모리해제를 할 수 ㅇ벗는 상황에 대해 설명해보세요. GC는 해결할 수 있을까요? (Explain the use case when ARC won't help you to release memory (but GC will)?)

- ARC는 강한 순환 참조 상황에서 인스턴스를 해제하지 못하는 문제가 있습니다. 만약 어떤 두 클래스 인스턴스가 프로퍼티에 서로를 `강함 참조`로 저장하고 있을 때 `강한 순환 참조`가 발생할 수 있습니다.

- 순환참조 발생

```swift
class A {
  var b: B?

  deinit {
      print("A deinitialzied")
  }
}

class B {
    var a: A?

    deinit {
        print("B deinitialzied")
    }
}

var a = A()
var b = B()

a.b = b
b.a = a

a = A()
b = B()

// retain cycle
```

- weak로 순환참조 회피

```swift
class A {
  var b: B?

  deinit {
      print("A deinitialzied")
  }
}

class B {
    var a: A?

    deinit {
        print("B deinitialzied")
    }
}

var a = A()
var b = B()

a.b = b
b.a = a

a = A()
b = B()

// B deinitialzied
// A deinitialzied
```

</br>

## 49. DispatchGroup에 대해서 설명해보세요. (Explain what is DispatchGroup?)

- DispatchGroup은 여러 스레드에 분배되어 있는 작업들을 그룹으로 묶어서 동기적으로 관리하기 위해 사용합니다.
- DispatchGroup에 추가될 때 `enter` 메서드를 통해 작업의 개수를 1 늘려주고, 작업이 끝날 때 DispatchGroup에서 빠져나오면서 `leave` 메서드를 통해 작업의 개수를 하나 줄여줍니다.
- 작업의 개수가 0이되면 `notify` 메서드가 실행되면서 모든 작업이 끝났을 때에 대한 처리를 수행할 수 있습니다.
- enter, leave, notoify를 이용해서 여러개의 비동기 작업들이 전부 끝날 때 작업을 수행하게 할 수도 있습니다.

```swift
// https://stackoverflow.com/questions/49376157/swift-dispatchgroup-notify-before-task-finish
let group = DispatchGroup()
let queueImage = DispatchQueue(label: "com.image")
let queueVideo = DispatchQueue(label: "com.video")

group.enter()
queueImage.async(group: group) {
    sleep(2)
    print("image")
    group.leave()
}

group.enter()
queueVideo.async(group: group) {
    sleep(3)
    print("video")
    group.leave()
}

group.notify(queue: .main) {
    print("all finished.")
}
```

</br>

## 50. DispatchWorkItem의 장점이 무엇인가요? (What are the benefits of using DispatchWorkItem in Swift?)

- DispatchWorkItem을 사용하면 DispatchQueue에 등록할 작업을 `캡슐화`할 수 있습니다.

```swift
let queue = DispatchQueue(label: "custom")
let workItem = DispatchWorkItem {
  print("task is running")
}
queue.async(execute: workItem)
```

- DispatchWorkItem을 사용하면 `cancel`, `wait` 등 제공되는 메서드를 사용해서 쉽게 작업에 대한 동작을 지정할 수 있습니다.

```swift
class SearchViewController: UIViewController, UISearchBarDelegate {
  // We keep track of the pending work item as a property
  private var pendingRequestWorkItem: DispatchWorkItem?

  func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
      // Cancel the currently pending item
      pendingRequestWorkItem?.cancel()

      // Wrap our request in a work item
      let requestWorkItem = DispatchWorkItem { [weak self] in
          self?.resultsLoader.loadResults(forQuery: searchText)
      }

      // Save the new work item and execute it after 250 ms
      pendingRequestWorkItem = requestWorkItem
      DispatchQueue.main.asyncAfter(deadline: .now() + .milliseconds(250),
                                    execute: requestWorkItem)
  }
}
```

</br>

## 51. Concurrent와 Serial Queue가 async, sync와 함께 사용되는 상황에 대해서 설명해보세요. (Explain usage of Concurrent vs Serial Queues with async and sync blocks)

- `concurrent + sync`: DispatchQueue에 작업이 `sync`하게 등록되기 때문에 작업을 등록하는 스레드는 작업을 등록하고 `작업이 마쳐질 때까지 기다립니다`. DispatchQueue에 등록된 작업은 `concurrent`하게 처리되기 때문에 작업의 종료여부와 상관없이 `할당 가능한 스레드가 있다면 큐에 있는 작업을 스레드에 할당`시켜 작업을 시작합니다.
- `concurrent + async`: DispatchQueue에 작업이 `async`하게 등록되기 때문에 작업을 등록하는 스레드는 등록한 작업이 끝나길 기다리지 않고 곧바로 다음 코드를 실행합니다. DispatchQueue에 있는 작업은 `concurrent`하게 처리되기 때문에 작업의 종료여부와 상관없이 `할당 가능한 스레드가 있다면 큐에 있는 작업을 스레드에 할당`시켜 작업을 시작합니다.
- `serial + sync`: DispatchQueue에 작업이 `sync`하게 등록되기 때문에 작업을 등록하는 스레드는 작업을 등록하고 `작업이 마쳐질 때까지 기다립니다`. DispatchQueue에 등록된 작업은 `serial`하게 처리되기 때문에 `현재 작업에 대한 처리가 끝나면` 큐에 등록된 다음 작업을 스레드에 할당해 시작합니다.
- `serial + async`: DispatchQueue에 작업이 `async`하게 등록되기 때문에 작업을 등록하는 스레드는 등록한 작업이 끝나길 기다리지 않고 곧바로 다음 코드를 실행합니다. DispatchQueue에 등록된 작업은 `serial`하게 처리되기 때문에 `현재 작업에 대한 처리가 끝나면` 큐에 등록된 다음 작업을 스레드에 할당해 시작합니다.

</br>

## 52. @escaping에 대해서 설명해보세요.

- @escaping은 함수의 인자로 전달된 클로저를 함수가 종료된 후에 실행될 수 있도록 하는 속성입니다.

```swift
func completionTest(completion: @escaping () -> Void) {
  DispatchQueue.global().async {
      completion()
  }
  print("1")
}

completionTest {
    print("end")
}
// 1
// end
```

</br>

## 53. @objc와 dynamic의 차이에 대해서 설명해보세요. (What's the difference between marking a method as @objc vs dynamic, when would you do one vs the other?)

- dynamic을 통해 선언된 멤버변수나 객체는 Objective-C를 사용해서 동`적으로 디스패치`됩니다. 동적 디스패치는 런타임에 클래스가 연관 클래스들의 메서드 구현체들 중 어떤 클래스의 구현체를 사용하지 결정하는 것을 의미합니다.
- @objc는 `스위프트의 API를 Objective-C에서` 사용할 수 있도록 하는 속성이니다.

</br>

## 54. Extension은 메서드를 Override 할 수 있을까요?

- Extension에서는 메서드를 `override 할 수 없습니다`.
- Extension은 `새로운 함수`를 추가하기 위한 기능이지 기존 함수를 대체하기 위한 기능은 아닙니다.

</br>

## 55. RunLoop에 대해서 설명해보세요.

- RunLoop는 스레드 당 하나씩 생성되어서 Thread에 작업이 생기면 처리하고, 아닐 때는 대기시키는 역할을 합니다.
- RunLoop는 `메인 스레드를 제외`한 스레드에서는 자동으로 실행되지 않고 `개발자가 직접` 실행시켜주어야 합니다.
- RunLoop를 실행하면 실행되는 동안 도착한 `EventSource`(input source, timer source)를 실행합니다.
- RunLoop는 `한 번만` 실행되고 실행이 끝나면 대기상태로 돌아갑니다.

</br>

## 56. autoreleasepool에 대해서 설명해보세요.

- autorelease는 참조 카운트의 감소를 `즉시하지 않고 예약`을 할 수 있게하는 키워드입니다. 예약된 release는 autoreleasepool에 등록되고, `autoreleasepool 인스턴스가 해제되면` 등록되어 있던 예약된 release들이 모두 실행됩니다.
- 만약 함수가 끝나기 전에 메모리에서 해제되어야 하는 인스턴스가 있다면 autorelease pool로 명시적으로 인스턴스의 참조 카운트를 줄여줄 수 있습니다.

```swift
func useManyImages() {
  let filename = pathForResourceInBundle

  for _ in 0 ..< 5 {
      for _ in 0 ..< 1000 {
          let image = UIImage(contentsOfFile: filename) // 5000개 생성
      }
  }
  // 5000개 해제
}

func useManyImages() {
  let filename = pathForResourceInBundle

  for _ in 0 ..< 5 {
    autoreleasepool {
      for _ in 0 ..< 1000 {
          let image = UIImage(contentsOfFile: filename) // 1000개 생성
      }
    } // 1000개 해제
  }
}
```

</br>

## 57. OperationQueue에 대해서 설명해보세요. DispatchQueue와는 어떤 것이 다른가요?

- `OperationQueue`는 작업을 나타내는 Operation 클래스의 실행을 관리하는 큐입니다. GCD와는 다르게 OperationQueue는 객체화된 작업을 큐에서 취소할 수 있고, 큐에 등록될 최대 작업의 개수를 설정하거나, 작업 간의 `의존성`(어떤 작업이 선행되어야 하는지)에 대한 정보도 설정할 수 있습니다. 즉, DispatchQueue보다 좀 더 고수준의 API를 제공합니다.
- OperationQueue는 `내부적으로 DispatchQueue`를 이용합니다.
- OperationQueue는 작업을 한번 객체화 하고 고수준 API를 지원하기 때문에 DispatchQueue보다 더 `많은 리소스를 사용`합니다. 따라서 복잡하고 의존성에 대한 설정이 필요한 작업이 아니라면 DispatchQueue를 사용하는 것이 더 좋을 수도 있습니다.

</br>

## 58. final 키워드를 클래스 앞에 붙이면 어떤 효과가 있을까요?

- 어떤 클래스의 프로퍼티나 메소드는 다른 자식 클래스로부터 override 될 수 있기 때문에, 이런 override된 메소드는 실제로 어떤 메소드를 실행할지 `vtable`을 한 번 탐색해서 결정하게됩니다. 즉, 컴파일 타임이 아닌 `런타임에 실제로 실행할 메소드가 결정`되는 것입니다. 이를 `dynamic dispatch`라고 합니다.
- final 키워드를 사용하면 해당 클래스, 프로퍼티, 메소드가 다른 클래스에 의해 상속되고 있지 않다는 것을 컴파일러에게 알려주기 때문에 `컴파일 타임에 어떤 메소드를 사용할지 바로 결정`할 수 있고, vtable을 거치지 않고 직접적으로 호출되기 때문에 성능상 더 좋은 퍼포먼스를 낼 수 있습니다.

</br>

## 59. vtable에 대해서 설명해보세요.

- vtable은 `가상 메소드 테이블`로 `컴파일 타임에 생성`되어 메소드가 호출되었을 때 사용할 구현체를 `런타임에 특정`할 수 있게 해줍니다.
- vtable은 `메소드 구현체의 주소를 배열로` 가지고 있습니다.
- 클래스마다 `vtable`이 존재합니다.

</br>

## 60. 프로퍼티 옵저버에 대해 설명해보세요.

- 프로퍼티 옵저버는 저장 프로퍼티의 값이 `변화하는 것을 관찰`하기 위해 사용합니다.
- `willSet`과 `didSet`을 사용해서 프로퍼티의 값이 변화할 때 함께 실행할 작업을 정의할 수 있습니다.
- willSet은 새로 변화될 값을 `newValue`라는 프로퍼티로 제공하고, didSet은 변화되기 전 값을 `oldValue`라는 프로퍼티로 제공합니다.

```swift
class A {
  var name: String {
      willSet {
          print("\(name) will be changed to \(newValue)")
      }
      didSet {
          print("\(oldValue) is changed to \(name)")
      }
  }

  init(name: String) {
      self.name = name
  }
}

let a = A(name: "A")
a.name = "B"
// A will be changed to B
// A is changed to B
```

- 연산프로퍼티는 부모 클래스의 연산 프로퍼티를 오버라이딩하는 경우만 프로퍼티 옵저버를 추가할 수 있습니다.

</br>

## 60. Property Wrapper에 대해 설명해보세요.

- 프로퍼티 래퍼는 여러 프로퍼티에 대해 반복되는 코드를 `재사용`할 수 있도록 해주는 기능입니다.
- `@propertyWrapper`로 구조체를 정의하고 내부에 프로퍼티에 대한 동작을 정의해두면, 프로퍼티를 선언할 때 프로퍼티 래퍼를 키워드로 붙여 미리 정의한 동작을 재사용할 수 있습니다.

```swift
@propertyWrapper
struct TwelveOrLess {
    private var number = 0
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 12) }
    }
}
struct SmallRectangle {
  @TwelveOrLess var height: Int
  @TwelveOrLess var width: Int
}

var rectangle = SmallRectangle()
print(rectangle.height)
// Prints "0"

rectangle.height = 10
print(rectangle.height)
// Prints "10"

rectangle.height = 24
print(rectangle.height)
// Prints "12"
```

</br>

## 61. 고차함수 중 flatMap과 compactMap의 차이를 설명해보세요.

- `compactMap`은 1차원 배열에서 각 요소에 대해 `nil을 제거`하고 `옵셔널 바인딩`을 한 결과를 배열로 만들어 반환합니다.
- `flatMap`은 1차원 배열에서는 `nil을 제거`하고 `옵셔널 바인딩`을 한 결과를 배열로 만들어 반환합니다.
- `flatMap`은 2차원 배열에서는 배열의 요소들을 `1차원으로 합친 배열`을 반환하고 nil의 제거와 옵셔널 바인딩은 하지 않습니다.

```swift
let test = [1, nil, 2, nil, 3, nil]
print(test.compactMap({ $0 }))
// [1, 2, 3]

let test = [1, nil, 2, nil, 3, nil]
print(test.flatMap({ $0 }))
// [1, 2, 3]

let test = [[1, nil], [2, nil], [3, nil]]
print(test.flatMap({ $0 }))
// [Optional(1), nil, Optional(2), nil, Optional(3), nil]

let test = [[1, nil], [2, nil], [3, nil]]
print(test.flatMap({ $0 }).compactMap({ $0 }))
// [1, 2, 3]

let test = [[1, nil], [2, nil], [3, nil]]
print(test.compactMap({ $0 }).flatMap({ $0 }))
// [Optional(1), nil, Optional(2), nil, Optional(3), nil]
```

</br>

## 62. High Order Function에 대해서 설명해보세요.

- 고차함수는 함수를 인자로 받는 함수입니다.

```swift
func test(execution: () -> Void) {
  execution()
}
```

</br>

## 63. 함수형 프로그래밍은 무엇인가요? Swift는 함수형 프로그래밍 언어인가요?

- 함수형 프로그램밍은 `순수 함수`를 기반으로 하는 프로그래밍 패러다임입니다. 순수 함수는 어떤 입력에 대해 `항상 같은 출력`을 만드는 함수를 의미합니다. 즉, 외부에 영향을 주거나 받는 `side effect`가 없습니다.
- 스위프트는 함수형 프로그래밍 언어이면서 동시에 객체 지향 프로그램밍 언어의 특징인 상속, 은닉, 캡슈화, 추상화 등을 제공하는 멀티 패터다임 언어입니다.

</br>

## 64. 1급 객체(혹은 1급 시민)에 대해서 설명해보세요. Swift에는 어떤 1급 객체들이 있나요?

- 1급 객체는 `함수의 인자로 전달되거나 반환 값으로 사용할 수 있는` 객체를 의미합니다.
- 또 1급 객체는` 변수나 상수에 할당` 할 수 있는 객체입니다.
- 스위프트는 기본 타입들과 함수나 클로저까지 모두 1급 객체에 해당합니다.

## 65. Optional은 내부적으로 어떻게 구현되어 있나요?

- Optional은 associated value를 가지는 `enum`으로 구현되어 있습니다. 값이 존재할 때는 some에 저장된 값을 반환하고, 값이 존재하지 않으면 nil을 반환합니다.

```swift
@frozen
enum Optional<Wrapped> {
  case none
  case some(Wrapped)
}
```

</br>

## 66. Swift에서 참조 타입을 말해보세요.

- 클래스, 함수, 클로저가 모두 참조 타입입니다.

</br>

## 67. String은 왜 subscript로 접근할 수 없을까요?

- String을 구성하는 각 문자들은 여러문자가 합성된 `Unicode Scalar` 로 이루어져 있습니다. 따라서 한 문자가 같은 크기의 메모리를 가지지 않습니다.

```swift
// https://docs.swift.org/swift-book/LanguageGuide/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID285
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e followed by ́
// eAcute is é, combinedEAcute is é
```

</br>

## 68. Result 타입에 대해서 설명해보세요.

- Result는 `success와 failure case` 를 가지는 enum입니다.
- failure 케이스의 associated value는 반드시 `Error 프로토콜`을 채택해야합니다.
- 네트워크와 같이 실패할 가능성이 있는 작업의 성공여부와 결과를 쉽게 표현할 수 있습니다.

```swift
// https://www.hackingwithswift.com/articles/161/how-to-use-result-in-swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in
  switch result {
  case .success(let count):
      print("\(count) unread messages.")
  case .failure(let error):
      print(error.localizedDescription)
  }
}
```

</br>

## 69. some 키워드에 대해서 설명해보세요.

- some은 `opqaue result type`입니다.
- 함수 내부에서 반환되는 타입을 외부에서 명확하게 알 수 없도록합니다.

```swift
// Error. 구체타입을 명시하지 않음
func someList() -> Collection {
  return [1,2,3]
}

// OK. 구체타입을 알 수는 없지만 Collection을 채택하는 타입이 반환될 것을 보증함
func someList() -> some Collection {
  return [1,2,3]
}
```

</br>

## 70. KVC에 대해서 설명해보세요.

- KVC는 `Key-Value Coding`으로 객체의 값을 직접 사용하지 않고 KeyPath를 이용해 `간접적`으로 사용하고 수정하는 방법입니다.
- `{백슬래시(\)}.{타입}.{keypath}` 로 keypath를 만들어 사용할 수도 있습니다.

```swift
struct A {
    var data = "Data"
}

var aInstance = A()
print(aInstance[keyPath: \.data]) // Data
print(aInstance.data) // Data
aInstance[keyPath: \.data] = "Data2"
print(aInstance[keyPath: \.data]) // Data2

let key = \A.data
print(aInstance[keyPath: key]) // Data2
```

</br>

## 71. KVO에 대해서 설명해보세요.

- KVO는 `Key-Value Oberving`으로 어떤 Key에 등록된 변수가 변경이 되는 것을 관찰하고 변경이 발생할 때마다 특정한 작업을 수행하기 위해 사용합니다.
- Objective-C 런타임에 의존하기 때문에 `NSObject`를 채택해야하고, 관찰할 프로퍼티에는 `@objc` 와 `dynamic` 키워드를 붙여야 합니다.

```swift
class Obj: NSObject {
    @objc dynamic var data = "Data"
}

let obj = Obj()
let observer = obj.observe(\.data, options: [.new, .old]) { _, changeInfo in
    print("\(changeInfo.oldValue) has been changed to \(changeInfo.newValue)")
}

obj.data = "Data2"
// Optional("Data") has been changed to Optional("Data2")
```

## Questions Source

- https://www.fullstack.cafe/interview-questions/swift
- https://github.com/JeaSungLEE/iOSInterviewquestions
