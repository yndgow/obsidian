# Life Cycle

## 1. Life Cycle 이란?

앱 또는 프로그램 내의 객체나 컴포넌트가 생성되고 소멸되는 과정을 기술하는 것을 생명주기(Life Cycle)라고 합니다. 특히 UI 프레임워크나 앱 개발에서, 위젯이나 컴포넌트의 생명주기는 매우 중요한 개념입니다.

## 2. StatelessWidget 과 StatefulWidget의 생명 주기
StatelessWidget은 상태를 가지지 않는 위젯입니다. 그래서 생명주기가 간단합니다.
- **Constructor** - StatelessWidget 인스턴스가 생성됩니다.
- **build** - 위젯이 빌드되어 화면에 UI를 표현합니다..

![](https://i.imgur.com/XILzUjM.png)


StatefulWidget은 상태를 가지는 위젯입니다. 그래서 생명주기가 복잡합니다.

- **StatefulWidget 생명 주기**
    
    1. **Constructor**: **`StatefulWidget`** 인스턴스가 생성됩니다.
    2. **createState**: 새로운 **`State`** 객체를 생성합니다. 이 메서드는 Flutter 프레임워크에 의해 호출됩니다.
    3. **initState**: **`State`** 객체가 초기화될 때 호출됩니다. 여기서 일회성 초기화를 수행합니다.
    4. **didChangeDependencies**: 의존성이 변경될 때 호출됩니다. 예를 들어, **`BuildContext`**를 사용하여 상위 위젯으로부터 데이터를 가져올 때 호출될 수 있습니다.(BuildContext의 종속성이나 부모 위젯에 연관된 데이터를 의미합니다)
    5. **build**: 위젯이 빌드되어 화면에 UI를 표현합니다.
    6. **setState**: 상태가 변경될 때마다 호출되어 **`build`** 메서드를 다시 실행합니다. 상태 변경을 트리거합니다.
    7. **didUpdateWidget**: 부모 위젯이 변경되어 현재 위젯의 설정이 변경될 때 호출됩니다.
    8. **dispose**: **`State`** 객체가 위젯 트리에서 제거될 때 호출됩니다. 여기서 리소스 해제와 같은 정리 작업을 수행합니다.

    이러한 생명주기 메서드들을 이해하고 올바르게 사용하는 것은 Flutter 앱 개발에서 중요한 부분입니다. 특히, 리소스 관리, 성능 최적화, 그리고 UI 업데이트와 관련된 문제를 해결하기 위해 이러한 메서드들을 적절하게 활용할 수 있어야 합니다.


![](https://i.imgur.com/Eh7cQBO.png)



```dart
import 'package:flutter/material.dart';  
  
void main()=>(  
  runApp(MyApp())  
);  
  
class MyApp extends StatefulWidget {  
  // 1. 생성자　－　인스턴스가　생성  
  const MyApp({super.key});  
  // 2. 새로운 State 객체를 생성합니다.  
  @override  
  State<MyApp> createState() => _MyAppState();  
}  
  
class _MyAppState extends State<MyApp> {  
  int _count = 0;  
  
  @override  
  void initState() {  
  
    super.initState();  
    print('initState() 호출 - 3');  
  }  
  
  // 호출 시점은 크게 두가지  
  // 1. initState() 호출 후에 바로 한 번 호출 됩니다.  
  // 2. 위젯이 사용하는 데이터가 변경이 되면 이 메서드는 자동으로 호출  
  // 단 !!! BuildContext의 종속성(Theme, Locale)이나 부모 위젯에 연관된 데이터를 의미합니다.  
  @override  
  void didChangeDependencies() {  
    super.didChangeDependencies();  
    print('didChangeDependencies() 호출 - 4');  
  }  
  
  // 화면을 그릴 때 호출 build()는 여러 상황에서 호출 될 수 있습니다.  
  // 상태가 변경되어 화면을 다시 그릴 때, 부모 위젯이 변경 되었을 때, 테마나, 디바이스에 화면 방향이 변경되었을 때  
  @override  
  Widget build(BuildContext context) {  
    print('build() 호출 - 5');  
    return MaterialApp(  
      home: Scaffold(  
        body: Center(  
          child: GestureDetector(  
              onTap: (){  
                setState(() {  
                  _count++;  
                  print('호출됨');  
                });  
              },  
              child: Text('반가워 $_count', style: TextStyle(fontSize: 40),)),  
        ),  
      ),  
    );  
  }  
  // 상태가 변경 될 떄 마다 호출 또는 setState() 호출 시  
  @override  
  void setState(VoidCallback fn) {  
    print('setState() 호출 - 6');  
    super.setState(fn);  
  }  
  // 부모 위젯이 변경되어 현재 위젯의 설정이 변경되어야 될 때 호출됩니다.  
  @override  
  void didUpdateWidget(covariant MyApp oldWidget) {  
    print('didUpdateWidget() 호출 - 7');  
    super.didUpdateWidget(oldWidget);  
  }  
  // State 객체가 위젯 트리에서 제거 될 때 호출됩니다.  
  // 여기서는 보통 리소스 해제와 같은 작업을 추가합니다.  
  @override  
  void dispose() {  
    super.dispose();  
  }  
}
```



![](https://i.imgur.com/o0lc9N4.png)
