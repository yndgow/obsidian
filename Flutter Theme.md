
## 1. Theme

Flutter의 Theme는 앱 전체의 색상, 스타일, 그래픽 디자인 언어 등을 중앙에서 관리할 수 있게 해주는 기능입니다. Theme을 사용하면 앱 전체의 디자인 및 레이아웃을 일관되게 유지하면서 손쉽게 변경할 수 있습니다.

일반적으로 MaterialApp 위젯에서 theme 속성을 통해 앱 전체에 대한 테마를 많이 설정하고 있습니다.

```dart
// 대표적으로 AppBar의 배경색, FloatingActionButton의 배경색 등에 사용됨
primary: Colors.orangeAccent, 
 // 대표적으로 Switch, Slider의 활성화 색상 등에 사용됨
secondary: Colors.greenAccent,
// 플러터에서 직접적으로는 사용되지 않지만, 사용자 정의 스타일링에 사용하는 삼차 색상
tertiary: Colors.blueAccent,
```

