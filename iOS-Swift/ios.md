## 1. Bounds 와 Frame의 차이점을 설명해보세요.

- Frame의 자신의 `상위 뷰의 좌표계`를 기준으로 위치를 표현하고, Bounds는 `자기자신의 좌표계`를 기준으로 위치를 표현합니다.
- Frame의 사이즈는 뷰의 x 좌표 최소, 최대값, y 좌표으로 최소, 최대값으로 설정합니다. 반면의 Bounds는 `뷰 자체의 크기`로 사이즈를 정합니다.
- Frame의 위치를 변경하면 `뷰가 이동`하지만, Bounds는 `뷰포트가 이동`합니다. 따라서 50pt 만큼 x좌표를 증가시키면 실제로는 50pt 만큼 왼쪽으로 이동한 것 처럼 보입니다.

<br/>

## 2. Bounds를 사용하는 예시가 어떤게 있을까요?

- `스크롤 뷰`는 스크롤 될 때마다 스크롤 뷰의 Bounds를 업데이트 합니다. 만약 왼쪽으로 스와이프해서 스크롤하면 bounds의 x좌표가 증가합니다.

<br/>

## 3. 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명해주세요.

- 개발환경에서 시뮬레이터를 사용할 경우, `세 손가락 이상의 터치 제스쳐`는 사용할 수 없습니다.
- 시뮬레이터에서는 `카메라, 마이크, 전화, 센서`를 사용할 수 없습니다.
- 앱 백그라운드 전환, 터치 등 기본적인 기능은 시뮬레이터에서도 사용할 수 있습니다.
- `다크모드`, `개발자용 네트워크 설정`도 시뮬레이터에서도 사용할 수 있습니다.

<br/>

## 4. 앱 화면의 콘텐츠를 표시하는 로직과 관리를 담당하는 객체를 무엇이라고 하는가?

- `UIViewController`입니다. 뷰 컨트롤러는 UIKit으로 구성된 앱의 뷰 계층관계를 관리하고 뷰를 그리는 로직을 담고 있습니다.

<br/>

## 5. App thinning에 대해서 설명하시오.

- 앱 시닝은 앱이 설치될 때 앱스토어와 운영체제가 앱이 설치되는 `디바이스의 특성에 맞게` 앱을 설치하는 최적화 기술입니다.
- 앱 시닝은 세 가지 기술 요소로 이루어져 있습니다.
- 먼저 `슬라이싱`은 앱을 구성하는 여러 버전의 실행 가능한 코드와 리소스들 중 앱을 설치할 `디바이스 스펙에 맞는 버전들만 골라서 설치`하는 것을 의미합니다. 앱을 앱스토어에 올리게되면 앱스토어가 여러 버전의 파일들을 준비하고 다운로드시 디바이스에 맞게 슬라이싱합니다.
- 다음은 `주문형 리소스`는 앱을 설치할 때 모든 리소스를 다 설치하는 것이 아니라 일부는 앱 스토어에 저장해두고 사용자가 필요로할 때만 다운로드 하는 방식입니다.
- 마지막으로 `비트코드`는 앱스토어에 앱을 올릴 때 기계어로 구성된 바이너리 파일이 아니라 그 전 단계인 비트코드로 업로드 하는 것을 의미합니다. 비트 코드로 된 파일 중 사용자 환경에 맞는 비트코드들만 다시 컴파일해 바이너리를 만들 수 있습니다.

<br/>

## 6. 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

- UIApplicationMain이 실행되면 `UIApplication` 객체가 생성됩니다.
- 또한 AppDelegate와 SceneDelegate 객체도 생성합니다.
- 마지막으로 앱의 루트가되는 window 객체를 생성하고 최초 뷰 컨트롤러를 할당합니다.

<br/>

## 7. UIApplication 객체는 어떤 일을 하나요?

- UIApplication은 UIApplicationMain에서 만들어지는 싱글톤 객체입니다. UIApplication은 최초에 런루프를 만들고 AppDelegate에게 delegate를 위임합니다. 
- 시스템에서 발생하는 이벤트가 UIKit에 의해 UIEvent객체로 만들어지면 UIApplication.shared.sendEvent()를 통해 해당 이벤트를 이벤트 큐에 전달합니다.

<br/>

## 8. UIApplicationDelegate는 어떤 종류의 메서드들을 포함하고 있나요?

- 앱을 시작하고 앱의 `생명주기`가 변화될 때마다 상황에 상응하는 메서드가 있습니다.
- `백그라운드`에서 다운로드 작업이 진행되어야할 때의 메서드가 있습니다.
- 앱의 `Scene`이 새로 생성되거나 지워질 때 호출되는 메서드도 있습니다.
- APN 등록처럼 앱의 실행이 시작되면서 등록해야하는 `서비스` 작업들에 대한 메서드가 있습니다.

<br/>

## 9. 앱 생명주기와 관련된 UIApplicationDelegate 메서드를 말해보세요.

- `applicationDidBecomeActive`: 앱이 active 상태가 되면 호출됩니다.
- `applicationWillResignActive`: 앱이 inactive 상태로 전환되기 직전에 호출됩니다.
- `applicationDidEnterBackground`: 앱이 백그라운드 상태로 들어가면 호출됩니다.
- `applicationWillEnterForeground`: 앱이 백그라운드에서 화면으로 들어오기 직전에 호출됩니다.
- `applicationWillTerminate`: 앱이 종료되기 직전에 호출됩니다.

<br/>

## 10. 앱이 In-Active 상태가 되는 시나리오를 설명하시오.

- 시리가 켜지거나 전화알림, 알러트가 뜨는 상황 등 사용자로부터 이벤트를 받지 못하는 상황에서 inactive상태가 됩니다.

<br/>

## 11. scene delegate에 대해 설명하시오.

- iOS 13부터 iOS는 Multiple Window를 지원하기 시작했습니다. 한 프로세스가 여러개의 화면을 가질 수 있는 것인데요, 이 때문에 UI 생명주기를 앱 전체에 대해 관리하는 것이 아니라, 각 창에 대해 관리할 필요가 있어졌습니다. SceneDelegate는 이를 위해 각 화면마다 만들어져서 AppDelegate가 가지고 있던 UI생명주기와 관련된 메서드들을 처리하게 됩니다.

<br/>

## 12. App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.

- Not running은 앱이 아직 실행되지 않은 상태, Inactive는 실행되고 있지만 이벤트를 받을 수 없는 상태, Background는 백그라운드에서 코드를 실행중인 상태, Suspended는 백그라운드에서 코드를 실행중이지 않은 상태를 의미합니다.

<br/>

## 13. iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

- UIKit입니다.

<br/>

## 14. Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.

- 스위프트에서 사용하는 String, Double과 같은 기본타입들과 Array, Dictionary같은 콜렉션타입이 들어가 있고 기본적인 기능을 제공하는 프레임워크입니다.

<br/>

## 15. Delegate란 무언인지 설명하고, retain 되는지 안되는지 그 이유를 함께 설명하시오.

- Delegate는 자신의 역할을 다른 누군가에게 위임하는 것입니다. 일반적으로 프로토콜로 위임할 동작들의 인터페이스를 적용한 뒤, 이 프로토콜을 위임을 받을 타입에 채택시킵니다. 그리고 위임을 해줄 타입에 weak var delegate를 만들어두고 특정한 메소드를 호출할 때 delegate에 구현되어 있는 메소드를 호출하면, 이를 위임받아 프로토콜을 채택하는 타입은 해당 메소드에 대한 구현체를 가지고 있으므로 대신 요청한 작업을 처리하게 됩니다.
- Delegate 변수는 항상 weak var로 선언하기 때문에 retain이 발생하지 않습니다.

<br/>

## 16. NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.

- notificationCenter는 싱글톤 객체로 Notification을 보낼 객체들을 addObserver로 이곳에 등록합니다. Notification을 보낼 때는 post 함수를 호출하면 post와 함께 주어진 정보를 NotificationCenter에 관리되는 객체들에게 모두 보내게됩니다.
- 옵저버 패턴이지만 중간 브로커 역할을 하는 NotificationCenter가 있기 때문에 한번에 여러 객체에게 어떤 정보를 보내고자할 때 브로드캐스팅의 목적으로 사용할 수 있습니다.

<br/>

## 17. UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

- UI와 관련된 모든 작업은 앱의 Main 스레드에서 처리됩니다.

<br/>

## 18. App Bundle의 구조와 역할에 대해 설명하시오.

- App Bundle은 실행가능한 파일, 코드, 리소스로 이루어져 있습니다. 앱 샌드박스 안에는 Documents, Library, Tmp 등이 있고, 이중에서 Library 안에는 Application Support, Caches, Preferences 등의 디렉토리가 있습니다.

<br/>

## 19. 각 디렉토리를 더 자세히 설명해주세요.

- Documents는 유저가 생성한 문서나 데이터를 저장하는 공간입니다. 사용자가 직접 접근할 수 있고 설정에 따라서 삭제도 가능합니다. iCloud에 백업이 되는 디렉토리입니다.
- Library는 유저의 파일이나 임시파일을 저장하게 됩니다. 이중 Application Support는 앱의 기능을 유지하기 위해 관리되는 데이터들이 저장되고 코어데이터의 기본 저장 경로가 되기도 합니다. iCloud에 백업이 됩니다.
- Library의 Caches 디렉토리는 임시적으로 보관할 파일들이 저장됩니다. iCloud에는 백업되지 않습니다.
- Caches 내부에는 Snapshot 디렉토리도 있습니다. 사용자가 백그라운드로 들어갈 때 시스템은 마지막 뷰에 대한 스냅샷을 찍어 이곳에 저장해두고 Suspended였던 앱이 재실행되면 런치스크린이 아닌 스냅샷을 보여주어서 재시작되지 않는 것 처럼 보이게합니다.
- Preferences 디렉토리는 앱의 설정 정보들이 저장됩니다.

<br/>

## 20. 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

- UIViewController입니다. 그리고 UIViewController의 상위클래스는 UIResponder입니다.
- UIViewController의 역할은 뷰를 업데이트하고 뷰에서 전달되는 사용자 이벤트와 상호작용하는 것이 있습니다.

<br/>

## 21. 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.

- Xib를 만들어 뷰를 잡고 관련된 코드를 UIView를 상속하는 클래스를 정의하거나, 클래스로 정의해 내부에서 레이아웃까지 잡아줄 수도 있습니다.

<br/>

## 22. View 객체에 대해 설명하시오.

- UIView는 UIResponder를 상속해 구현된 화면을 보여주는 컨테이너 역할을 하는 객체입니다. 이 객체를 통해 이벤트가 전달되고 draw함수를 호출해 화면을 그리기도 합니다.

<br/>

## 23. UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오.

- layer의 타입은 CALayer입니다. CA는 Core Animation 프레임워크의 약자이고 UIKit보다 한단계 낮은 저수준의 인터페이스를 제공하는 프레임워크입니다. 애니메이션이나 좀 더 복잡한 화면에 대한 처리는 CALayer를 통해 이루어지고, CALayer는 별도의 스레드에서 GPU를 사용해 화면을 그리게됩니다.

<br/>

## 24. UIWindow 객체의 역할은 무엇인가?

- UIWindow는 직접적으로 시각적인 내용을 나타내지는 않지만 화면을 구성하는 모든 뷰들의 부모가되는 컨테이너 역할을 하는 객체입니다.

<br/>

## 25. UINavigationController의 역할이 무엇인지 설명하시오.

- UINavigationController는 스택처럼 화면들을 쌓아서 화면간 이동을 관리하는 컨테이너입니다.

<br/>

## 26. TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.

- 인덱스마다 어떤 셀을 사용할지 반환하는 cellForRowAt이 있고, 섹션마다 표시할 셀의 개수를 반환하는 numberOfRowsInSection이 있습니다.

```swift
 func tableView(UITableView, cellForRowAt: IndexPath)
 func tableView(UITableView, numberOfRowsInSection: Int)
```

<br/>

## 27. 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

- IBOulet을 만들어두고 delegate가 실행될 때 인자로 전달되는 TableView의 인스턴스를 IBOutlet과 비교하거나, tag를 이용해서 구분할 수 있습니다.

<br/>

## 28. setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.

- setNeedsLayout은 뷰의 위치와 크기를 업데이트하는 layoutSubviews를 다음 업데이트 사이클에 호출하도록 예약하는 메서드입니다. setNeedsDisplay는 뷰의 내용을 그리는 draw 메서드를 다음 업데이트 사이클에 호출하도록 예약하는 메서드입니다.

<br/>

## 29. NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.

- NSCache는 내부적으로 캐시정책을 가지고 있어서 저장하는 오브젝트의 개수나 cost를 정해두고 초과되면 정책에 따라 오브젝트를 삭제합니다. 딕셔너리는 내부적으로 따로 정책이 없습니다. 또한 NSCache는 thread safe하기도 합니다.

<br/>

## 30. NSCache 정책이 어떻게 구성되어있는지 아는지?

- LRU와 LFU의 하이브리드라고 알고있습니다. 공식문서에 따로 나와있지 않지만 libs 코드를 보았을 때 자주 참조된 데이터는 지우지 않은 채로 가장 오래전 데이터부터 순차적으로 지워주고 있습니다.

https://github.com/gnustep/libs-base/blob/master/Source/NSCache.m

<br/>

## 31. NSCache의 cost랑 오브젝트 개수가 무엇이 다른지?

- cost는 오브젝트의 개수이고 cost는 오브젝트의 크기입니다. set할 때 오브젝트에 대한 cost를 함께 받을 수 있고, cost가 한계값으로 정해둔 크기보다 커지면 캐시 정책에 따라 데이터를 제거합니다.

<br/>

## 32. URLSession에 대해서 설명하시오.

- URLSession은 네트워크 통신을 제공하는 기본 프레임워크의 클래스입니다.
- SessionTask를 만들고 여기에 통신에 대한 설정과 콜백을 정의해서 넘기면 네트워크 통신이 완료되었을 때 클로저가 실행됩니다.

<br/>

## 33. URLSessionConfiguration 종류

- default는 기본 상태, emphemral은 캐시를 지우지 않을 때, backgound는 앱이 백스라운드에서 다룬로드를 받을 수 있게합니다.

<br/>

## 34. URLDownloadTask와 URLSessionDataTask의 차이

- dataTask는 NSData 타입으로 데이터를 내려받기 때문에 로컬 저장소에는 저장하지 않습니다. 따라서 백그라운드 세션을 지원하지 않습니다. downloadTask로 내려받은 파일은 temp 디렉토리에 저장됩니다. 따라서 백그라운드 세션을 지원합니다.

<br/>

## 35. prepareForReuse에 대해서 설명하시오.

- prepareForReuse는 테이블 뷰나 컬렉션 뷰에서 셀을 재사용할 때 호출되는 메서드입니다. 화면에서 사라진 셀은 리유저블 큐에 저장되고 cellForItemAt 에서 dequeue되었을 때 다시 재사용됩니다. prepareForReuse는 cellForItemAt 이 호출되기 전에 호출되어서 셀의 설정들을 초기화할 수 있도록 도와줍니다.

<br/>

## 36. 다크모드를 지원하는 방법에 대해 설명하시오.

- 간단하게는 Asset에서 다크모드와 일반 모드에 대한 설정을 해줄 수 있습니다. Any, Dark, Light에 대한 색상을 각각 설정할 수 있습니다. 또, userInterfaceStyle 프로퍼티를 통해 현재 디바이스가 다크모드인지 라이트모드인지 알 수 있기 때문에 이를 이용해서 인터페이스를 세팅해줄 수 있을 것 같습니다.

<br/>

## 37. ViewController의 생명주기를 설명하시오.

- loadView, viewDidLoad, viewWillAppear, viewWillLayoutSubviews, viewDidLayoutSubviews, viewDidAppear, viewWillDisappear, viewDidDisappear 가 있습니다.
- loadView에서는 뷰 컨트롤러의 기본 view를 생성하고 할당합니다. viewDidLoad는 뷰 컨트롤러가 메모리에 로드된 직후에 호출되고, viewWillAppear는 화면이 나타나기 직전에 호출됩니다. viewWillLayoutSubviews는 뷰의 바운드가 설정되고 레이아웃이 변경되기 직전에 호출됩니다. 그리고 레이아웃이 설정되면 viewDidLayoutSubviews가 호출됩니다. 그리고 화면이 나타나면 viewDidAppear가 호출됩니다. 화면이 사라질 때는 사라지기 직전에 viewWillDisappear가 호출되고 사라진 후는 viewDidDisappear가 호출됩니다.

<br/>

## 38. TableView와 CollectionView의 차이점을 설명하시오.

- 테이블 뷰는 기본적으로 상하스크롤의 목록만을 지원합니다. 반면에 콜렉션 뷰는 flow layout이나 compositional layout으로 목록의 레이아웃을 더 다양하게 구성할 수 있습니다.

<br/>

## 39. 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)

- NSLayoutContraint, Anchor, 비주얼 포맷 세가지 방법이 있습니다.
- NSLayoutContraint는 item, attribute, multiplier, contraint를 지정해서 두 객체간의 레이아웃 관계를 정의하는 방법입니다.
- NSLayoutAnchor는 NSLayoutContraint를 더 간단한 API로 만든 것으로 Anchor와 contraint를 통해 객체간의 레이아웃 관계를 정의할 수 있습니다.
- 비주얼 포맷은 아스키 문자열을 통해 레이아웃을 시각적으로 표현하여 레이아웃을 정의하는 방법입니다.

<br/>

## 40. hugging, resistance에 대해서 설명하시오.

- hugging은 최대 크기에 대한 제한이고, resistance는 최소 크기에 대한 제한입니다. 따라서 hugging의 우선순위가 다른 뷰들보다 높은 뷰는 더 커지지 않으려고 하고, resistence의 우서순위가 다른 뷰들보다 높은 뷰는 더 작아지지 않으려고 합니다.
- 예를 들어 길이가 정해진 스택 뷰 안에 두 UILabel이 있다고 했을 때, 왼쪽 뷰의 hugging priority가 더 높은 뷰는 스택뷰를 채우기 위해 길이를 늘리지 않고, 오른쪽 뷰가 길이를 늘려 스택뷰를 채우게 됩니다.
- 반대로 resistence priority가 더 높은 뷰는 더 작아지지 않으려고 하기 때문에 스택뷰의 크기가 두 UILabel의 컨텐츠를 모두 표시하지 못하는 크기라면, resistence priority가 더 낮은 뷰의 크기가 줄어듭니다.

<br/>

## 41. Intrinsic Size에 대해서 설명하시오.

- Intrinsic Conetent Size는 UIButton, UILabel 등에서 사용되어서 뷰 내부의 컨텐츠에 따라 계산되는 뷰의 크기를 의미합니다. 커스텀 뷰에서는 이 프로퍼티를 오버라이딩해서 크기를 계산해주고 invalidIntrinsicContentSize를 호출해주어야합니다.

<br/>

## 42. 스토리보드를 이용했을때의 장단점을 설명하시오.

- 스토리보드를 사용하면 시각적으로 뷰를 확인할 수 있기 때문에 오토레이아웃이나 뷰의 구성을 바로바로 학인하고 쉽게 수정할 수 있다는 장점이 있습니다. 하지만 뷰의 재사용이 어렵고 화면이 많아지면 프로젝트를 로드하는 속도가 느려진다는 점, 그리고 협업시에 여러 사람이 스토리보드를 수정하게되면 충돌이 쉽게 발생한다는 단점이 있습니다.

<br/>

## 43. Safearea에 대해서 설명하시오.

- safearea는 iOS 디바이스중 상단 노치 영역과 하단 홈 바 영역을 말합니다. 기본적으로 앱이 이 영역들을 침범하지 못하도록 safearea를 기준으로 오토레이아웃을 설정합니다.

<br/>

## 44. Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

- 문자에 대한 제약을 설정할 때 Left는 항상 문자의 시작을 왼쪽에 두지만 Leading은 언어에 따라서 우측에서 좌측으로 읽는 언어는 시작을 오른쪽에, 좌측에서 우측으로 읽는 언어는 시작을 왼쪽에 둡니다.

<br/>

## 45. UIControl에 대해서 설명해주세요.

- UIControl은 UIView를 상속받는 객체로 사용자의 인터렉션에 대한 기능을 제공합니다. UIControl은 상태 정보를 제공하고 addTarget으로 UIControl 객체에서 어떤 이벤트가 발생했을 때 처리할 메소드를 지정할 수 있습니다.

<br/>

## 45. CollectionViewLayout을 커스텀하게 정의할 때 prepare 메서드가 언제 불리고 어떤 역할을 하는지

- 콜렉션 뷰가 컨텐츠를 처음 표시하거나 뷰가 변경되어서 레이아웃이 무효화되면 호출됩니다. prepare 메서드가 호출되면 레이아웃 객체에게 레이아웃을 업데이트 하도록 시키게됩니다. 레이아웃이 업데이트 되기 시작하면 콜렉션 뷰가 이 메서드를 호출해서 정의한 레이아웃 객체가 레아이웃을 잡을 수 있게 합니다.

<br/>

## 46. #selector의 역할이 무엇인지.

- Objective-C 런타임으로 실행되는 메서드를 지정하기 위해서 사용합니다.

<br/>

## 47. Autoresizing과 Autolayout의 차이

- Autoresizing mask는 어떤 뷰의 바운드가 변경되었을 때, 그 하위 뷰들의 프레임을 어떻게 변경할지 정의합니다. autoresizing mask는 위, 아래, 왼쪽, 오른쪽에 대해 설정할 수 있고 설정된 방향은 상위뷰의 bounds가 변화함에 따라 함께 변화하게 됩니다. 예를 들어서 오른쪽에 대해 Autoresizing mask를 설정하면 화면의 크기가 변화되어서 가로 길이가 늘어났을 때, 설정한 서브뷰의 가로 길이가 함께 늘어나게 됩니다. 설정이 되어있지 않은 면들은 기존 크기를 그대로 유지합니다.

<br/>

## 48. super.viewDidLoad()를 제거하면 어떤 일이 일어나는지?
- 상위 클래스의 viewDidLoad를 호출하지 않아도 문제는 일어나지 않지만, UIKit의 viewDidLoad가 언제 구현이 바뀔지 모르고, 내부에 중요한 초기화 코드가 들어갈 수도 있기 때문에 당장 영향이 없더라도 super.viewDidLoad를 호출하는 것이 바람직하다고 생각합니다.

<br/>

## 49. UIResponder에 대해 설명해주세요.
- UIResponder는 모든 UIView의 상위 객체이면서 이벤트를 받고 처리하거나 다른 UIResponder 객체에게 전달하는 역할을 합니다. UIKit은 first responder에게 발생한 이벤트를 전달하고 UIResponder는 해당 이벤트를 자신이 처리할 수 있다면 처리하고, 처리할 수 없다면 next 프로퍼티에 할당된 다음 Responder에게 전달합니다.

<br/>

## 50. Responder Chain은 그럼 뭔가요?
- UIKit이 모든 UIResponder를 엮어서 관리하는 체인입니다. 각 리스폰더는 next프로퍼티로 자신의 다음 리스폰더를 참조하고 있습니다.

<br/>

## 51. 그럼 기본적으로 클래스별로 이벤트가 전달되는 순서가 어떤지
- UIView, UIViewController, UIWindow, UIApplication, UIApplicationDelegate 순으로 이벤트가 전달됩니다. 만약 어떤 뷰나 뷰 컨트롤러가 상위뷰에 속해져있는 뷰라면 해당 뷰나 뷰 컨트롤러에 이벤트를 전달합니다.

<br/>

## 52. Responder Chain을 임의로 변경하려면 어떻게 해야할까요?
- 특정한 뷰의 next 프로퍼티를 오버라이딩해서 다음 리스폰더를 지정할 수 있고, becomeFirstResponder 메서드를 사용해서 특정한 뷰를 firstResponder로 만들 수 있습니다. 이때는 이벤트가 first responder에게 전달됩니다. 

<br/>

## 53. 그럼 이벤트는 어떤 형태로 전달되는지? 터치 이벤트는 다른게 있는지?
- 이벤트는 UIEvent 객체로 전달됩니다. 터치 이벤트는 UITouch 객체로 관리되고 UIEvent 객체를 통해 접근할 수 있습니다. 터치 이벤트 객체는 터치된 시간, 터치된 영역, 터치 강도, 터치 위치 등의 정보를 포함하고 있습니다.

<br/>

## 54. UIControl에서 이벤트가 발생하면 해당 이벤트를 처리하기까지의 과정을 설명해주세요.
- UIControl은 addTarget 메서드를 정의할 수 있게 합니다. 이때 target과 action을 인자로 전달할 수 있습니다. target은 어떤 객체이던지 들어갈 수 있지만 일반적으로는 이벤트를 처리할 뷰 컨트롤러를 지정합니다. 만약 target에 nil이 들어가면 UIControl은 발생한 이벤트에 대한 처리를 구현하고 있는 리스폰더를 리스폰더 체인을 통해 찾아냅니다. 
- action은 이벤트를 처리할 메서드에 대한 시그니처를 나타냅니다. 만약 이벤트가 발생하면 UIControl 객체는 이 메서드를 호출하고 UIApplication이 호출 메시지를 받아 리스폰더 체인에서 이 메서드를 찾아 이벤트를 전달합니다. @IBAction이 이런 메서드를 식별할 수 있도록 합니다.

<br/>

## 55. UI 작업을 메인스레드에서 처리해야하는 이유
- UI는 변경을 트리거하는 이벤트가 발생하자마자 변경되는 것이 아니라 런루프의 한 사이클 끝에 변경됩니다. 만약 여러 스레드에서 UI작업을 처리하게 되면 각각 다른 런루프에서 작업을 처리하게 되고, 뷰가 화면에 그려지는 시점이 제각각이 되거나 레이아웃에 대한 계산이 의도했던 것과 다를 수 있을 것 같습니다.

<br/>

## 56. UIViewController의 상위 클래스들을 모두 말하고 설명해주세요.
- UIViewController는 UIResponder를 상속하고, UIResponder는 NSObject를 상속합니다. 
- UIResponder는 이벤트를 받고 리스폰더 체인을 구성할 수 있게 합니다. NSObject는 Objective-C의 루트 클래스로 NSObject를 상속해 Objective-C 런타임에 대한 인터페이스나 기능을 사용할 수 있도록 합니다.

<br/>

## 57. 앱이 처음 시작되면 어떤 일들이 일어나는지 설명해주세요.
- main 함수가 실행됩니다.
- main 함수는 UIApplicationMain 함수를 호출합니다.
- UIApplicationMain은 UIApplication 인스턴스를 생성합니다.
- 그리고 Info.plist에서 필요한 데이터를 로드합니다. main Nib 파일을 여기서 찾아 로드합니다.
- UIApplication은 AppDelegate 인스턴스를 생성하고 UIApplication을 위임합니다. 
- UIApplication은 RunLoop를 생성합니다.
- 준비가 완료되면 AppDelegate의 didFinishlaunchingWithOptions를 호출합니다.
- 세션에 대한 설정이 완료되면 SceneDelegate의 willConnectToSession이 호출됩니다.

![image](https://user-images.githubusercontent.com/46087477/150353961-db9c84a8-2eb3-4d42-b108-690943f718de.png)
