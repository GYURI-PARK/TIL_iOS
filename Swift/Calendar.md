## ✨ struct Calendar

Swift에서 구조체 `Calendar`는 연도의 시작, 길이 및 분할이 정의된 시간 측정 시스템에 관한 정보를 캡슐화한 것으로, 달력에 관한 정보와 특정 날짜 단위의 범위를 결정하거나 절대 시간에 일정 단위를 추가하는 등의 **날짜 계산**을 지원한다.

</br>
</br>

### 👀 Getting Weekday Symbols
> 달력에서의 주중 요일 목록으로, 달력의 locale에 맞게 지역화되어있다.

#### veryShortWeekdaySymbols

```swift
print(Calendar.current.veryShortWeekdaySymbols)
```
> ["S", "M", "T", "W", "T", "F", "S"]

</br>
</br>

### 👀 Extracting Components
> 달력에서 원하는 날짜를 추출한다.

#### dateComponents(_:from:)
> 달력의 시간대를 사용하여 날짜의 모든 날짜 구성 요소를 반환한다.

```swift
func dateComponents(
    _ components: Set<Calendar.Component>,
    from date: Date
) -> DateComponents
```
> components : 반환하는 요소들의 집합, date: 기준이 되는 날짜
> 지정된 날짜의 components를 반환한다.

```swift
Calendar.current.dateComponents([.year, .month], from: today)
```
> today가 오늘 날짜인 경우 .year = 2023, .month = 10이 된다.

</br>
</br>

### 👀 Calculating Dates from Components
> 구성요소들로부터의 날짜 계산 함수들을 제공한다.

#### date(from:)
> 구성 요소의 생성된 날짜를 반환한다.

```swift
let components = Calendar.current.dateComponents([.year, .month], from: today)
print(Calendar.current.date(from: components)!)
```
> 오늘 날짜의 해당하는 달의 첫번째 날이 출력된다.

#### date(byAdding:value:to:wrappingComponents:)
> 지정된 날짜에 특정 구성 요소를 더한 날짜를 나타내는 새로운 Date를 반환한다.

```swift
Calendar.current.date(byAdding: .day, value: day, to: startOfMonth())!
```
> startOfMonth()에서 반환된 날짜에 day만큼의 일 수를 더하여 새로운 날짜를 계산하고, 이를 나타내는 새로운 Date 객체를 반환한다.
