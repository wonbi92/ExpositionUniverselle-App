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
|<img width = 220, src = "https://i.imgur.com/ecg4zXF.gif">|<img width = 220, src = "https://i.imgur.com/Y7VkzGK.gif">|<img width = 220, src = "https://i.imgur.com/B77H29Z.gif">|

## 👀 Diagram

### 🧬 Class Diagram

![](https://i.imgur.com/ZU4fEDg.png)

 
## 🗂 폴더 구조
> Modal : 앱 구동 로직에 필요한 모델<br>
> View : 화면을 구성하는 뷰<br>
> Controller : 화면의 이벤트와 전환을 컨트롤하는 컨트롤러
```
Expo1900
├── Expo1900
│   ├── appDelegate
│   ├── sceneDelegate
│   ├── Model
│   │   ├── DTO
│   │   │   ├── ExpoInformation
│   │   │   └── Entry
│   │   └── Extension
│   │       └── JSONDecoder+Extension
│   ├── Controller
│   │   ├── ExpoMainViewController
│   │   ├── EntryViewController
│   │   └── DetailViewController
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
- `ExpoMainViewController`
- `EntryViewController`
- `DetailViewController`
    
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

## 🏃🏻 기술적 도전

[⚙️ JSON Decoding](https://github.com/wonbi92/ios-exposition-universelle/wiki/4.-Challenge#%EF%B8%8F-json-decoding)<br><br>
[⚙️ DTO](https://github.com/wonbi92/ios-exposition-universelle/wiki/4.-Challenge#%EF%B8%8F-dto)<br><br>
[⚙️ UITableView](https://github.com/wonbi92/ios-exposition-universelle/wiki/4.-Challenge#%EF%B8%8F-uitableview)<br>

## 🚀 트러블 슈팅
[📌 TextView Scrolling](https://github.com/wonbi92/ios-exposition-universelle/wiki/5.-Troubleshooting#-textview-scrolling)<br><br>
[📌 JSONDecoder](https://github.com/wonbi92/ios-exposition-universelle/wiki/5.-Troubleshooting#-jsondecoder)

## 📝 일일 스크럼

[일일 스크럼 바로가기](https://github.com/wonbi92/ios-exposition-universelle/wiki/3.-Daily-Scrum)

## 🔗 참고 링크

- [UITableView](https://developer.apple.com/documentation/uikit/uitableview)
- [Table Views](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)
- [Filling a Table with Data](https://developer.apple.com/documentation/uikit/views_and_controls/table_views/filling_a_table_with_data)
- [Configuring the Cells for Your Table](https://developer.apple.com/documentation/uikit/views_and_controls/table_views/configuring_the_cells_for_your_table)
- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder)
- [Using JSON with Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/using_json_with_custom_types)
- [Encoding and Decoding Custom Types](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types)
- [NumberFormatter](https://developer.apple.com/documentation/foundation/numberformatter)<br>
