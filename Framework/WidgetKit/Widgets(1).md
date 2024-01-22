# ✨ Build SwiftUI views for widgets
> [참고) WWDC20 영상](https://developer.apple.com/videos/play/wwdc2020/10033/)

</br>

## 💡 SwiftUI in widgets

* SwiftUI와 WidgetKit을 사용하면 홈 화면에서 적절한 시간에 표시할 뷰의 타임라인을 제공할 수 있다.
> Provide a timeline of content for the home screen to display

</br>

* SwiftUI의 유연성 덕분에 위젯을 작성하고 iOS 및 macOS에 모두 배포할 수 있다.
> Create widgets for macOS and iOS with a single implementation
<img width="60%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2c62def3-7931-4e61-aa83-4c43a5d33de5">

</br>
</br>

## 💡 WidgetKit의 4가지 구성요소
> 프로젝트에 Widget Extension을 추가하면 Widget과 관련된 Swift파일이 생성된다.
> 그리고 생성된 파일은 아래 4가지 구성요소를 포함하고 있다. 

</br>

### 1. The Timeline Entry

- `Timeline Entry`는 위젯 뷰의 모델 객체이다.
- 이는 최소한 날짜 매개변수를 포함해야 하고, 다른 매개변수들은 필요에 따라 타임라인 엔트리에 추가할 수 있다.

```swift
struct SimpleEntry: TimelineEntry {
    let date: Date
    let providerInfo: String
}
```

> providerInfo를 사용하여 `TimelineProvider`와 관련된 정보를 저장하고 나중에 위젯에 표시한다.

</br>
</br>

### 2. The Widget View

- 기본적인 SwiftUI View라 생각하면 된다.

```swift
struct ViewSizeWidgetEntryView : View {
    var entry: Provider.Entry

    var body: some View {
        VStack {
            Text("\(Int(UIScreen.main.bounds.width)) x \(Int(UIScreen.main.bounds.height))")
                .font(.system(.title2, weight: .bold))
            
            Text(entry.providerInfo)
                .font(.footnote)
        }
        .containerBackground(for: .widget) {
            Color.green
        }
    }
}
```
> `entry`매개변수를 사용해 Timeline Entry의 데이터 (providerInfo)를 위젯 UI에 표시한다.

</br>
</br>

### 3. The Timeline Provider

- `Timeline Provider`는 특정 타임스탬프에서 위젯에 표시할 내용에 관한 정보를 시스템에 제공한다.
- 타임라인 프로바이더는 `TimelineProvider 프로토콜`을 따라야 한다.
- 이 프로토콜은 다음과 같은 3가지 메서드 요구사항을 가지고 있다.

```swift
struct Provider: TimelineProvider {
    
    typealias Entry = SimpleEntry

  // 1. placeholder(in:) 메서드
  // 위젯이 준비될 때까지 대기하는 동안 시스템에 더미 데이터를 제공하여 플레이스홀더 UI를 렌더링
    func placeholder(in context: Context) -> Entry {
        return SimpleEntry(date: Date(), providerInfo: "placeholder")
    }

  // 2. getSnapshot(in:completion:) 매서드
  // 시스템이 위젯 갤러리에서 위젯을 렌더링하는 데 필요한 데이터를 제공
    func getSnapshot(in context: Context, completion: @escaping (Entry) -> ()) {
        let entry = SimpleEntry(date: Date(), providerInfo: "snapshot")
        completion(entry)
    }

  // 3. getTimeline(in:completion:) 매서드
  // 타임라인 프로바이더에서 가장 중요한 메서드로, 현재 시간을 기준으로 현재 및 필요에 따라 미래의 타임라인 엔트리 배열을 제공하여 위젯을 업데이트
    func getTimeline(in context: Context, completion: @escaping (Timeline<Entry>) -> ()) {
        let entry = SimpleEntry(date: Date(), providerInfo: "timeline")
        let timeline = Timeline(entries: [entry], policy: .never)
        completion(timeline)
    }
}
```

<img width="30%" alt="image" src="https://github.com/GYURI-PARK/TIL_iOS/assets/93391058/2d0b449b-a9ae-4cb6-9f38-e32be038d708">

> Widget snapshot in widget gallery

</br>
</br>

### 4. The Widget Configuration

- Widget Configuration은 방금 구현한 모든 내용을 통합하는 곳이다.

```swift
struct ViewSizeWidget: Widget {
    let kind: String = "ViewSizeWidget"

    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            if #available(iOS 17.0, *) {
                ViewSizeWidgetEntryView(entry: entry)
                    .containerBackground(.fill.tertiary, for: .widget)
            } else {
                ViewSizeWidgetEntryView(entry: entry)
                    .padding()
                    .background()
            }
        }
        .configurationDisplayName("View Size Widget")
        .description("This is an example widget.")
        .supportedFamilies([
            .systemSmall,
            .systemMedium,
            .systemLarge,
        ])
    }
}
```
