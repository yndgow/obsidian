
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

```dart
import 'package:flutter/material.dart';  
  
// 하나의 코드 베이스에 light 모드와 다크 모드 적용 시키기  
ThemeData initThemeData({required Brightness brightness}) {  
  
  if(brightness == Brightness.light){  
    return ThemeData(  
      brightness: Brightness.light,  
      colorScheme: ColorScheme.light(  
        primary: Colors.orange, // 주요 색상  
        secondary: Colors.green, // 보조 색상  
      ),  
    );  
  }else{  
    return ThemeData(  
      brightness: Brightness.dark,  
      colorScheme: ColorScheme.dark(  
        primary: Colors.blueAccent, // 주요 색상  
        secondary: Colors.greenAccent, // 보조 색상  
      ),  
    );  
  }  
}
// TextTheme textTheme(Color textColor) { 
// return TextTheme( 
// displayLarge: GoogleFonts.archivo(fontSize: 24.0, color: textColor), 
// displayMedium: GoogleFonts.archivo( 
// fontSize: 20.0, color: Colors.black, fontWeight: FontWeight.bold), 
// displaySmall: GoogleFonts.archivo( 
// fontSize: 20.0, color: Colors.black, fontWeight: FontWeight.bold), 
// bodyLarge: GoogleFonts.archivo(fontSize: 20.0, color: textColor), 
// bodyMedium: GoogleFonts.archivo(fontSize: 18.0, color: textColor), 
// bodySmall: GoogleFonts.archivo(fontSize: 16.0, color: textColor), 
// titleLarge: GoogleFonts.archivo(fontSize: 16.0, color: textColor), 
// titleMedium: GoogleFonts.archivo(fontSize: 14.0, color: textColor), 
// titleSmall: GoogleFonts.archivo(fontSize: 12.0, color: textColor), 
// ); 
// } 

// AppBarTheme appBarTheme() { 
// return AppBarTheme( 
// centerTitle: false, 
// color: Colors.white, 
// elevation: 0.0, 
// iconTheme: iconTheme(), 
// titleTextStyle: GoogleFonts.nanumGothic( 
// fontSize: 16, 
// fontWeight: FontWeight.bold, 
// color: Colors.black, 
// ), 
// ); 
// } 
// 

// IconThemeData iconTheme() { 
// return const IconThemeData( 
// color: Colors.orange, 
// ); 
// }

```


💡 **ThemeData에서 대표적으로 설정 가능한 유형**

1. **Typography**:
    - **`textTheme`**: 전체적인 텍스트 스타일을 설정할 수 있습니다. 예를 들어, 제목, 부제목, 본문 텍스트 등 다양한 텍스트 스타일을 정의할 수 있습니다.
2. **Button Themes**:
    - **`buttonTheme`**: 버튼의 기본 스타일 및 모양을 설정할 수 있습니다.
    - **`elevatedButtonTheme`**, **`outlinedButtonTheme`**, **`textButtonTheme`**: 각각의 버튼 유형에 대한 테마를 정의합니다.
3. **AppBar**:
    - **`appBarTheme`**: 앱바의 스타일, 색상, 높이, 밝기 등을 설정할 수 있습니다.
4. **FloatingActionButton**:
    - **`floatingActionButtonTheme`**: 플로팅 액션 버튼의 배경색, 모양, 높이 등을 설정할 수 있습니다.
5. **InputDecoration**:
    - **`inputDecorationTheme`**: 입력 필드의 외관, 모양, 경계 스타일 등을 설정할 수 있습니다.
6. **Icon Themes**:
    - **`iconTheme`**: 아이콘의 색상, 크기, 불투명도를 설정할 수 있습니다.
    - **`primaryIconTheme`**, **`accentIconTheme`**: 주요 및 강조 아이콘에 대한 테마를 정의합니다.
7. **Snackbar, Dialog, BottomSheet**:
    - **`snackBarTheme`**, **`dialogTheme`**, **`bottomSheetTheme`**: 각각의 위젯에 대한 스타일 및 모양을 설정할 수 있습니다.
8. **TabBar**:
    - **`tabBarTheme`**: 탭바의 색상, 지시자 크기 및 스타일을 설정할 수 있습니다.
9. **Scrollbar**:
    - **`scrollbarTheme`**: 스크롤바의 두께, 반경, 색상 등을 설정할 수 있습니다.
10. **Divider and DividerTheme**:
    - **`dividerTheme`**: 구분선의 색상, 공간, 두께 등을 설정할 수 있습니다.