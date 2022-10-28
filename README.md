# 🇫🇷 만국박람회

- SunnyCokkie와 Wonbi가 만든 1900년도 파리에서 개최된 만국박람회와 한국의 출품작에 대해 설명하는 만국박람회 앱입니다.

## 📖 목차
1. [팀 소개](#-팀-소개)
2. [GroundRule](#-ground-rule)
3. [Code Convention](#-code-convention)
4. [기능 소개](#-기능-소개)
5. [Diagram](#-Diagram)
6. [폴더 구조](#-폴더-구조)
7. [타임라인](#-타임라인)
8. [기술적 도전](#-기술적-도전)
9. [트러블 슈팅](#-트러블-슈팅)
10. [일일 스크럼](#-일일-스크럼)
11. [참고 링크](#-참고-링크)


## 🌱 팀 소개
 |[Wonbi](https://github.com/wonbi92)|[써니쿠키](https://github.com/sunny-maeng)|
 |:---:|:---:|
| <img width="180px" img style="border: 2px solid lightgray; border-radius: 90px;-moz-border-radius: 90px;-khtml-border-radius: 90px;-webkit-border-radius: 90px;" src="https://avatars.githubusercontent.com/u/88074999?v=4">| <img width="180px" img style="border: 2px solid lightgray; border-radius: 90px;-moz-border-radius: 90px;-khtml-border-radius: 90px;-webkit-border-radius: 90px;" src="https://avatars.githubusercontent.com/u/107384230?v=4">|

## 🤙 Ground Rule

[Ground Rule 바로가기](https://github.com/wonbi92/ios-exposition-universelle/wiki/1.-Ground-Rule)

## 🖋 Code Convention

[Code Convention 바로가기](https://github.com/wonbi92/ios-exposition-universelle/wiki/2.-Expo-Project-Code-convention)

## 🛠 기능 소개
|**만국박람회 메인 화면**|**한국의 출품작 목록**|**출품작 상세 페이지**|
 |:---:|:---:|:---:|
|<img width = 220, src = "https://i.imgur.com/ecg4zXF.gif">|<img width = 220, src = "https://i.imgur.com/EzQnKej.gif">|<img width = 220, src = "https://i.imgur.com/mO1KB7S.gif">|

|**메인 화면 - 세로방향만 지원**|**나머지 화면 - 모든방향 지원**|**다이나믹 타입 적용**|
  |:---:|:---:|:---:|
 |<img width = 300, src = "https://i.imgur.com/40Xk8Rg.gif">|<img width = 300, src = "https://i.imgur.com/iDCdtlB.gif">|<img width = 220, src = "https://i.imgur.com/cYe7sa6.gif">|
 


## 👀 Diagram

### 🧬 Class Diagram
![](https://i.imgur.com/sqzIA8S.png)

 
## 🗂 폴더 구조
> Model : 앱 구동 로직에 필요한 모델<br>
> View : 화면을 구성하는 뷰<br>
> Controller : 화면의 이벤트와 전환을 컨트롤하는 컨트롤러
```
Expo1900
├── Expo1900
│   ├── appDelegate
│   ├── sceneDelegate
│   ├── Model
│   │   ├── DTO/
│   │   │   ├── ExpoInformation
│   │   │   └── Entry
│   │   └── Extension
│   │       ├── JSONDecoder+Extension
│   │       └── String+Entension
│   ├── Controller/
│   │   ├── ExpoMainViewController
│   │   ├── EntryViewController
│   │   ├── EntryTableViewCell
│   │   ├── DetailViewController
│   │   └── ExpoNavigationController
│   ├── View
│   │   ├── Main
│   │   └── LaunchScreen
│   ├── info
│   └── Assets
└── Expo1900Tests
    └── Expo1900Tests
```

## ⏰ 타임라인

### 👟 Step 1
- `ExpoInformation` 구조체
- `Entry` 구조체

<details>
<summary>Details</summary>
<div markdown="1">

#### 1️⃣ `ExpoInformation` 구조체
 - 만국박람회 메인 화면에서 사용할 `exposition_universelle_1900` JSON파일 데이터를 가져오기위한 DTO 입니다.
#### 2️⃣ `Entry` 구조체
 - 한국의 출품작 화면에서 사용할 `items` JSON파일 데이터를 가져오기 위한 DTO 입니다.
 - `items` JSON파일의 경우, JS의 네이밍(snake_case)이 스위프트의 네이밍(camelCase)과 달라 `Entry` 구조체 내부에서 `CodingKey` 프로토콜을 채택한 `CodingKeys` 열거형을 사용해 JSON파일에 정상적으로 접근하도록 구현하였습니다.
    
</div>
</details>
    
### 👟 Step 2
- JSONDecoder+Extension
- ExpoMainViewController
- EntryViewController
- DetailViewController
    
<details>
<summary>Details</summary>
<div markdown="1">

#### 1️⃣ JSONDecoder+Extension
- `decode(_:, from:)` 메서드
    - Asset Name을 매개변수로 받아 JSONDecoder를 이용해 데이터를 디코딩하는 메서드를 구현하였습니다.

#### 2️⃣ ExpoMainViewController
엑스포에 대한 정보를 담은 포스터 stackView가 담긴 Scroll뷰로 보여줍니다.
- `viewDidLoad`
    - JSON포맷을 디코딩해 전역변수 `expoInformation`프로퍼티에 담습니다.
    - `buildExpoMainView` 메서드를 호출해 첫페이지를 draw합니다.
- `viewWillAppear`
    - 첫 시작뿐 아니라 다음페이지에서 뒤로 돌아올 때도 작동할 수 있도록 `buildNavigationBar` 메서드를 이곳에서 호출합니다.
- `buildNavigationBar`메서드 
    - 네비게이션 바의 title을 지정해주고, 네비게이션바를 숨깁니다.
- `buildExpoMainView`메서드
    - JSON포맷을 디코딩한 `expoInformation` 프로퍼티의 데이터를 이용해 메인뷰의 포스터의 `ImageView`, `label`의 정보를 지정합니다.
- `tapEntryButton`메서드
    - 버튼을 누르면 다음화면(뷰)을 push합니다.

#### 3️⃣ EntryViewController
한국의 출품작을 TableView로 보여줍니다.
- `viewDidLoad`
    - JSON포맷을 디코딩해 전역변수 entries 배열로 담습니다.
    - TableView의 DataSource와 Delegate를 자기자신으로 설정합니다.
    - `buildNavigationBar` 메서드를 호출해 첫페이지의 네비게이션 바를 build합니다.
- `buildNavigationBar`
    -  네비게이션 바의 title을 지정해주고, 네비게이션 바를 나타냅니다.
- `UITableViewDelegate`, `UITableViewDataSource` 프로토콜을 채택해 Table View를 draw합니다.
- `tableView(_:, didSelectRowAt:)` 메서드
    - 셀이 선택되면 상세페이지 화면으로 전환됩니다.
    - 선택된 작품의 정보를 다음화면의 변수에 담습니다.
- `tableView(_:,numberOfRowsInSection)` 메서드
    - entries 배열의 수만큼 테이블 뷰의 row를 생성합니다
- `tableView(_:,cellForRowAt) 메서드`
    - identifier에 맞는 셀을 생성하거나 재사용해 반환합니다
    - cell에 담긴 `imageView`, `label` 등에 entries 정보와 속성을 지정합니다. 

#### 4️⃣ DetailViewController
출품작의 상세내용을 `imageView`와 `textView`가 담긴 Scroll뷰로 보여줍니다.
- `viewDidLoad`
    - `buildNavigationBar`메서드와 `buildDetailView`메서드를 호출하여 화면을  draw합니다.
- `fetchEntryData`메서드
    - 이전 화면에서 선택된 출품작의 데이터를 가져옵니다.
- `buildNavigationBar`메서드
    - 네비게이션 바의 title을 지정해주고, 네비게이션바를 나타냅니다.
- `buildDetailView`메서드
    - `imageView`와 `textView`에 선택된 출품작의 이미지와 Description을 지정합니다.
</div>
</details>

### 👟 Step 3
- String+Extension
- ExpoMainViewController
- EntryViewController
- EntryTableViewCell
- DetailViewController
- ExpoNavigationController

<details>
<summary>Details</summary>
<div markdown="1">

#### 1️⃣ String+Extension
- `applyHangulAttribute() -> NSAttributedString`
    - 텍스트뷰에 한글 줄바꿈을 적용시키기 위해 `String`을 `NSAttributedString`으로 바꿔줍니다.

#### 2️⃣ ExpoMainViewController
- `init?(coder:)`
    - `expoInformation`프로퍼티의 데이터를 `JSONDecoder`통해 가져와 초기화 시킵니다.
- `viewWillAppear(_:)`
    - `portrait`상수를 통해 뷰가 나타날 때의 상태를 세로모드로 고정합니다.
    - `orientation`상수를 통해 현재 화면의 모드를 결정해 적용합니다. 
- `viewWillDisappear(_:)`
    - `orientation`상수를 통해 뷰가 사라지기 전 현재 현재 화면의 모드를 결정해 적용합니다. 
- `configureAttribute()`
    - 뷰의 각 구성요소들의 속성을 코드를 통해 정의합니다.
- `buildExpoMainView()`
    - 뷰의 구성요소들에 `expoInformation`프로퍼티의 데이터를 주입합니다.

#### 3️⃣ EntryViewController
- `init?(coder:)`
    - `entries`프로퍼티의 데이터를 `JSONDecoder`통해 가져와 초기화 시킵니다.
- `viewWillAppear(_:)` 
    - `orientation`상수를 통해 뷰가 나타날 때의 화면 모드를 결정해 적용합니다. 
- `tableView(_:, didSelectRowAt:)` 메서드
    - `instantiateViewController(identifier:, creator:)` 메서드를 이용해 스토리보드의 뷰컨트롤러를 커스텀 이니셜라이저를 이용해 초기화합니다.

#### 4️⃣ EntryTableViewCell
- `configureAttribute` 메서드
    - 셀의 각 구성요소들의 속성을 코드를 통해 정의합니다.

#### 5️⃣ DetailViewController
- `init?(entry:, coder:)`
    - `Entry` 타입의 데이터를 매개변수로 받아 `entry`프로퍼티의 데이터를 초기화 시킵니다.
- `init?(coder:)`
    - `fatalError()` 메서드를 이용해 사용하지 않는 이니셜라이저 임을 명시합니다. 
- `configureAttribute` 메서드
    - 뷰의 구성요소들의 속성을 코드를 통해 정의합니다.
- `configureImageViewConstraints` 메서드
    - 이미지 뷰의 오토레이아웃 제약조건을 코드를 통해 정의합니다.

#### 6️⃣ ExpoNavigationController
- `supportedInterfaceOrientations` 프로퍼티
    - 지원하는 가로세로 모드를 첫화면에서만 세로모드가 되도록 오버라이딩 합니다.

</div>
</details>

## 🏃🏻 기술적 도전
### ⚙️ JSON Decoding 

<details>
<summary>Details</summary>
<div markdown="1">
    
```swift
let decoder: JSONDecoder = JSONDecoder()
    guard let asset = NSDataAsset(name: asset) else { return }
        
    do {
        try decoder.decode(type, from: asset.data)
    } catch {
        print(error.localizedDescription)
    }
```
- 서버에서 전송받아오는 JSON 데이터(이번 프로젝트에서는 편의상 프로젝트 asset에 담겨져 있는 데이터)를 스위프트에서 사용가능하도록 decoding하는 작업입니다.
- JSON데이터는 스위프트의 문법과 달라, 스위프트에서 사용하려면 스위프트에 맞게 데이터를 바꿔주어야 합니다. 이 과정을 이 `JSONDecoder`라는 객체를 통해 진행할 수 있습니다.<br><br>
- 💡 이번 프로젝트에서는 프로젝트 asset에 담겨져 있는 만국박람회 데이터와 한국의 출품작 데이터를 스위프트에서 사용할 수 있도록 파싱하는 역할로 사용하였습니다.
    </div>    
</details>

### ⚙️ DTO
<details>
<summary>Details</summary>
<div markdown="1">    

```swift
struct Entry: Codable {
    let name: String
    let imageName: String
    let shortDescription: String
    let description: String
    
    private enum CodingKeys: String, CodingKey {
        case name
        case imageName = "image_name"
        case shortDescription = "short_desc"
        case description = "desc"
    }
}
```
- DTO는 계층간 데이터 교환을 위해 사용하는 객체입니다.
- MVC 패턴에서 Controller는 View와 Model(정확히는 받아오는 JSON데이터)사이의 데이터를 주고 받을 때 **DTO** 를 사용하기도 합니다. 
- 만약 DTO를 사용하지 않고 날것의 JSON데이터를 View에 넘겨주게 된다면, 민감한 데이터까지 모두 View에 노출될 수 있고, 데이터의 비즈니스 기능까지 모두 노출되게 됩니다.
- 또한 Model과 View 사이에 의존성이 생기는 문제도 있습니다.
- 이 DTO를 사용하면 받아올 JSON데이터를 캡슐화하고, UI 화면에서 사용하는 데이터만 선택적으로 보낼 수 있습니다.
- 또한 데이터를 이 DTO를 통해서 주고받게 됨으로써, Model과 View 사이에 의존성과 결합도를 낮출 수 있습니다. <br><br>
- 💡 이번 프로젝트에서는 프로젝트 asset에 담겨져 있는 만국박람회 데이터와 한국의 출품작 데이터를 다이렉트로 라벨에 넣지 않고 이 DTO객체를 통해 전달받도록 하였습니다.
    </div>    
</details>

### ⚙️ UITableView
<details>
<summary>Details</summary>
<div markdown="1">    
    
![](https://i.imgur.com/lxS0wJK.png)
- 테이블 뷰에 원하는 정보를 표시하고, 사용자의 행동에 적절히 반응하는 로직을 구현하기 위해 필요한 요소가 바로 `DataSource`와 `Delegate`입니다.
- `UITableView`는 이 두가지 객체가 없으면 정상적으로 동작하기 어렵기 때문에, 이 두가지 객체가 꼭 필요합니다. <br><br>
```swift
extension EntryViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        // 선택된 셀의 출품작 상세 페이지가 push되도록 구현
    }
}

extension EntryViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // 한 섹션에 출품작의 갯수만큼 셀이 존재하도록 구현
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath ) -> UITableViewCell {
        // 각 셀에 asset을 통해 받아온 출품작 데이터들이 나타나도록 구현
    }
}
```
- 💡 이번 프로젝트에서는 `DataSource`를 통해 테이블 뷰 셀에 알맞은 데이터가 삽입되도록 하였고, `Delegate`를 통해 사용자가 각 셀을 선택하였을 때 수행할 동작을 결정하는 방식으로 활용하였습니다.
</div>    
</details>

### ⚙️ Dynamic Type
<details>
<summary>Details</summary>
<div markdown="1"> 
    
```swift
titleLabel.numberOfLines = 0
titleLabel.font = .preferredFont(forTextStyle: .title1)
titleLabel.adjustsFontForContentSizeCategory = true
```
- 다이나믹 타입은 사용자가 선호하는 텍스트 크기를 선택할 수 있도록하여 유연성을 제공하는 기능을 합니다.
- Dynamic Type Sizes를 사용하기 위해서 Font Style을 TextStyle로 지정해줘야 합니다.
    -    코드로 작성할 때엔 `preferredFont(forTextStyle:)` 메서드를 사용해 폰트스타일을 수정할 수 있습니다.
    -    storyboard에서는 inspector에서 폰트스타일을 설정해줄 수 있습니다.
- 사용자가 텍스트 크기를 조절할 때 앱을 재실행 하지 않아도 Dynamic Type이 적용되도록 합니다.
    -    코드로 작성할 때엔 `adjustsFontForContentSizeCategory` 속성을 true로 지정합니다.
    -    storyboard에서는 inspector에서 `Automatically Adjust Font`속성의 체크박스를 체크합니다.<br><br>
- 💡 이번 프로젝트에서는 코드를 이용하여 `UIlabel`과 `UITextView`의 폰트에 다이나믹 타입이 적용되고, `Automatically Adjust` 하도록 설정하여 앱의 접근성을 높혔습니다.
</div>    
</details>



### ⚙️ instantiateViewController(identifier:creator:)
<details>
<summary>Details</summary>
<div markdown="1">
    
```swift
// 셀을 터치하면 새로운 뷰를 push합니다.
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    tableView.deselectRow(at: indexPath, animated: true)
        
    guard let detailViewController = storyboard?.instantiateViewController(
        identifier: "DetailViewController",
        creator: { coder in
            return DetailViewController(entry: self.entries[indexPath.row], coder: coder)
        }) else { return }
        
    navigationController?.pushViewController(detailViewController, animated: true)
    }

// 불러올 뷰 컨트롤러입니다.
final class DetailViewController: UIViewController {
    private let entry: Entry
    
    init?(entry: Entry, coder: NSCoder) {
        self.entry = entry

        super.init(coder: coder)
    }

    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```
- 기존 iOS 13.0이전에서는 외부에서 값을 주입시키기 위해 미리 이 값이 초기화가 되어 있어야 했습니다.
```swift
private var entry: Entry?
```
- 따라서, 다음과 같이 변수로 선언해야 했으며, 상황에 따라서는 옵셔널을 사용해야만 했습니다.
- 하지만, iOS 13.0 이후로는 `UIStoryboard`의 새로운 메서드인 `instantiateViewController(identifier:creator:)`를 활용할 수 있게 되었습니다.
- 이 메서드를 통해 스토리보드로 만든 뷰 컨트롤러의 커스텀 이니셜라이저를 불러올 수 있게 되었습니다.
- 주의할 점은, 커스텀 이니셜라이저를 구현함으로써 뷰 컨트롤러의 Required Initializer에서 `fatalError()`와 같은 메서드를 통해 사용해선 안된다는 것을 명시적으로 보여주어야 합니다. </br></br>
- 💡 이번 프로젝트에서는 뷰 컨트롤러 사이의 데이터 전달할 때, 전달받는 데이터를 초기화 단계에서 주입시켜 줌으로써, 변수가 아닌 상수로 값을 사용하게 하였고, 더불어 옵셔널을 사용하지 않아, 옵셔널 바인딩을 하지 않고 데이터를 활용하게 하여 코드의 가독성도 높이는 방법으로 사용하였습니다.
</div>
</details>



## 🚀 트러블 슈팅
### 📌 TextView Scrolling
<details>
<summary>Details</summary>
<div markdown="1">
    
|**Scrolling Enabled 속성 체크**|**Scrolling Enabled 속성 해제**|
 |:---:|:---:|
|<img width = 220, src = "https://i.imgur.com/zV8hQQX.gif">|<img width = 220, src = "https://i.imgur.com/O4xM2Be.gif">|

**문제 👻**
- ScrollView 위에 TextView를 올렸을 때, TextView 자체적으로 스크롤링되는 문제가 있었습니다.
- 더불어 TextView의 height가 정해지지 않아 ScrollView의 AutoLayout의 세로축이 모호하다는 경고가 뜨는 문제가 있었습니다.

**해결 🔫**
- TextView의 스크롤링 가능여부를 지정해주는 `Scrolling Enabled`속성을 해제해줍니다. <br>
    <img width = 220, src = "https://i.imgur.com/8RgIVMh.png">
- 만약, 코드로 이를 해제해주고자 한다면 다음 프로퍼티를 false로 지정하면 됩니다.
```swift
textView.isScrollEnabled = false
```
</div>
</details> 

### 📌 JSONDecoder
<details>
<summary>Details</summary>
<div markdown="1">    
    
```swift
extension JSONDecoder {
    static func decode<T: Decodable>(_ type: T.Type, from asset: String) -> T? {
        let decoder: JSONDecoder = JSONDecoder()
        guard let asset = NSDataAsset(name: asset) else { return nil }
        
        do {
            return try decoder.decode(type, from: asset.data)
        } catch {
            print(error.localizedDescription)
            return nil
        }
    }
}
```

**문제 👻**
- `JSONDecoder`로 `NSDataAsset`을 decoding을 해주는 작업을 한 번 이상 반복해야해서 재활용이 가능한 함수로 분리하고 싶었습니다.

**해결 🔫**
- `JSONDecoder`의 Extension에서 decode메서드를 static메서드로 구현했습니다
- `Generic Type(T)`을 이용해 decoding 할 타입을 매개변수 `Generic Type`을 받을 수 있도록 구현했습니다. 이 때 이 제네릭 타입은 `Decodable`프로토콜을 준수해야만 합니다.
- 매개변수로 받는 asset을 `NSDataAsset`의 name으로 직접받도록 했습니다. 이 name은 내부에서 `NSDataAsset`을 `guard let` 구문으로 가져올 때 사용됩니다.
- `do-catch` 구문으로 `JSONDecoder`의 `Decode`메서드를 이용해 decoding을 진행합니다.
    </div>
</details> 

### 📌 Dynamic Type 적용
<details>
<summary>Details</summary>
<div markdown="1">

| 문제의 화면 | <img width = 250, src= "https://i.imgur.com/ow41i2L.png"> |<img width = 250, src= "https://i.imgur.com/sbONiCH.png">|
| -------- | -------- | -------- |
| 수정 후 화면 | <img width = 250, src= "https://i.imgur.com/3qPtXGh.png"> |<img width = 250, src= "https://i.imgur.com/Y91IzRf.png">|

**문제 👻** 
- 첫번째 `ExpoMainView`에서 Dynamic Type의 가장 큰 글씨크기를 적용 했을 때, 위의 문제의 화면처럼 Button의 실제 사이즈는 커지지않고 Title이 담기는 라벨 사이즈만 커져서 위의 TextView의 영역을 침범하는 문제가 있었습니다

**해결 🔫**
- 버튼이 담긴 스택뷰의 alignment속성을 변경하여 해결했습니다. 버튼은 양 옆의 국기 이미지뷰와 함께 세로 스택뷰로 구성되어있습니다. 이 스택뷰의 alignment 속성을 fill에서 center로 수정해 문제를 해결했습니다.
</div>
</details>    

### 📌 각 뷰의 화면방향 지원
<details>
<summary>Details</summary>
<div markdown="1">

첫번째 뷰의 방향만 `portrait`으로 고정해주기 위해 `NavigationController`의 `supportedInterfaceOrientations`프로퍼티를 이용해 다음과 같은 코드를 구현했습니다.
```swift
class ExpoNavigationController: UINavigationController {
    override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return (topViewController as? ExpoMainViewController) != nil ? .portrait : .all
    }
}
```
**문제 👻** 
- 두번째 뷰에서 가로모드로 방향전환 후 그 상태로 navigationBar의 BackButton으로 첫번째 뷰로 다시 돌아올 때는 첫번재 뷰가 세로로 고정되어있지 않고 가로모드로 보여지는 문제가 있었습니다.

**해결시도(1) 🤜**
**supportedInterfaceOrientations 프로퍼티 사용**
```swift
  override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return .portrait
    }
```
- 첫번째 뷰의 viewController에서 `supportedInterfaceOrientations`프로퍼티를 `portrait`으로 고정하도록 구현해보았습니다.
- 문제는 해결되었지만 두번째 뷰에서 가로모드로 첫번째 뷰로 돌아갔다가 방향전환없이 다시 두번째뷰로 이동하면 아래와같이 네비게이션바가 정상작동하지 않는 또 다른 문제가 있었습니다.
 <img width = 400, src = "https://i.imgur.com/PLeITKh.png">
- 이 문제에 대한 저희의 견해는 네비게이션 바는 가로방향으로 설정하고 있는 반면에, 전체 뷰는 첫 화면에서 세로모드로 고정되어있어서 가로 모드를 인식하지 못해 첫 화면에서의 속성을 그대로 따라가기 때문이라고 생각했습니다.
- 저희가 원하는 방향의 UI도 아니고, 사용자에게 뒤로가기 버튼을 제대로 보여주지 않기에 이 방법을 채택하지 않았습니다.

**해결시도(2) 🤜** 
**AppDelegate에서 orientation 설정**
```swift
//앱딜리게이트에 구현
    var shouldSupportAllOrientation = true

    func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
        return shouldSupportAllOrientation ? .all : .portrait
    }

// 세로고정하고싶은 뷰컨에서 구현
let appDelegate = UIApplication.shared.delegate as! AppDelegate

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    appDelegate.shouldSupportAllOrientation = false
}

override func viewWillDisappear(_ animated: Bool) {
    super.viewWillDisappear(animated)
    appDelegate.shouldSupportAllOrientation = true
}
```
- 컨셉을 바꿔 `NavigationController`에서 화면방향을 설정해주지 않고 Appdelegate에서 설정해주는 방법을 적용해 보았습니다. 
- 두번째뷰에서 가로로 화면전환 후 backButton으로 돌아올 때 세로모드가 지원되지않는 문제가 있었습니다.
- 첫 번째 화면은 반드시 세로모드로 고정되게 하는 요구사항이 있었으므로, 이 방법역시 채택하지 않았습니다.
 
**해결 🔫**
`UIDevice.current.setValue(_:, forKey:)`로 현재방향 지정해주기
```swift
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
 
        let portrait = UIInterfaceOrientation.portrait.rawValue
        
        UIDevice.current.setValue(portrait, forKey: "orientation")
    }
```
- ExpoMainView의 `ViewWillAppear`메서드 내부에서 현재 디바이스의 방향을 portrait으로 지정해줍니다. 이렇게 하면 navigationBar의 backButton으로 화면으로 돌아갈 때도 세로모드만 지원됩니다. 
</div>
</details>    
 
### 📌 textView에서 lineBreak 적용    

<details>
<summary>details</summary>
<div markdown="1">  

**문제 👻**
> textView의 lineBreak 설정이 한글 지원을 완벽하게 하지 않음 
- 텍스트 뷰에서 lineBreakMode를 `.byWordWrapping`으로 적용하였음에도 줄바꿈이 제대로 일어나지 않는 문제가 있었습니다.

**해결시도(1) 🤜**
- `UITextView`대신 `UILabel` 사용을 고려해봤습니다.
    - 하지만, HIG에 따르면 다음과 같이 서술하고 있습니다.
    > Use a text view when you need to display text that’s long, editable, or in a special format. If you need to display a small amount of text, it’s simpler to use a label instead or a text field if the text is editable.

    - 이러한 이유로 긴 문장을 `UILabel`로 보여주는 것은 좋은 방법이 아니라 판단하여 적용하지 않았습니다.

**해결 🔫**
- `NSAttributedString`사용하기
```swift
extension String {
    func applyHangulAttribute() -> NSAttributedString {
        let paragraphStyle = NSMutableParagraphStyle()
        
        if #available(iOS 14.0, *) {
            paragraphStyle.lineBreakStrategy = .hangulWordPriority
        }
        
        let attributes: [NSAttributedString.Key: Any] = [
            .font: UIFont.preferredFont(forTextStyle: .body),
            .paragraphStyle: paragraphStyle
        ]
        
        return NSAttributedString(string: self, attributes: attributes)
    }
}
```
- `NSAttributedString`는 `String`에 텍스트의 속성을 저장하여 사용할 수 있는 구조체입니다.
- 이 구조체를 활용하여 iOS 14버전 부터 사용 가능한 한글 줄바꿈 속성(`.hangulWordPriority`)을 적용하여 textView에 보여지도록 구현해 보았습니다.    

 </div>
</details>


## 📝 일일 스크럼

[일일 스크럼 바로가기](https://github.com/wonbi92/ios-exposition-universelle/wiki/3.-Daily-Scrum)

## 🔗 참고 링크

[공식문서]
- [UITableView](https://developer.apple.com/documentation/uikit/uitableview)
- [Table Views](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)
- [Filling a Table with Data](https://developer.apple.com/documentation/uikit/views_and_controls/table_views/filling_a_table_with_data)
- [Configuring the Cells for Your Table](https://developer.apple.com/documentation/uikit/views_and_controls/table_views/configuring_the_cells_for_your_table)
- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder)
- [Using JSON with Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/using_json_with_custom_types)
- [Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)
- [NumberFormatter](https://developer.apple.com/documentation/foundation/numberformatter)
- [UIStoryboard](https://developer.apple.com/documentation/uikit/uistoryboard)
- [Scaling Fonts Automatically](https://developer.apple.com/documentation/uikit/uifont/scaling_fonts_automatically)
- [NSAttributedString](https://developer.apple.com/documentation/foundation/nsattributedstring)
- [UIInterFaceOrientation](https://developer.apple.com/documentation/uikit/uiinterfaceorientation)

[그 외 참고문서]
- [BoodstCourse-TableView](https://www.boostcourse.org/mo326/lecture/16860?isDesc=false)
- [Kodeco - Encoding and Decoding in Swift](https://www.kodeco.com/3418439-encoding-and-decoding-in-swift)
- [야곰닷넷-오토레이아웃 정복하기](https://yagom.net/courses/autolayout/)
