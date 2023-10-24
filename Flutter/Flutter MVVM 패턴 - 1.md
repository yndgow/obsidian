
## 1. 앱 아키텍처

앱 아키텍처는 애플리케이션의 전반적인 구조와 구성요소, 그리고 이러한 구성요소 간의 관계와 상호작용을 정의하는 청사진 또는 설계 원칙을 의미합니다. 쉽게 말해, 앱을 구축하는 데 필요한 구성요소와 이러한 구성요소들이 어떻게 함께 작동하고 연결되는지를 설명하는 방법론이라고 할 수 있습니다.


MVC, MVP, MVVM, VIPER 등 너무나 많은 아키텍처가 존재 한다. 하지만 원리는 거의 동일 하다.

- **역할 별로 레이어를 나눈다.**
- **각 레이어는 각자의 역할에만 집중하게 설계하고 자신 밖에 업무에서 가능한 신경을 끈다.**
- **각 레이어를 나누게 되면 수정 및 테스트 유지 보수가 용이하다.**


## 2. MVC 패턴

MVC (Model-View-Controller) 패턴은 오랫동안 사용되어온 소프트웨어 디자인 패턴 중 하나입니다. 웹 애플리케이션, 데스크톱 애플리케이션, 그리고 최근에는 모바일 애플리케이션에서도 널리 사용됩니다.

1. **가독성**: 각 구성요소(Model, View, Controller)가 독립적이기 때문에 코드의 구조가 명확해져서 가독성이 향상됩니다.
2. **확장성**: MVC 패턴은 기능의 확장이 필요할 때, 해당하는 부분만을 수정하면 되기 때문에 확장성이 좋습니다. 예를 들면, UI를 변경하고자 할 때 View만 수정하면 되고, 데이터 처리 방식을 변경하고자 할 때는 Model만 수정하면 됩니다.
3. **재사용성**: 각 구성요소가 독립적이므로 재사용이 용이합니다. 특히 Model은 다른 시스템이나 프로젝트에서도 재사용할 수 있습니다.
4. **분리와 집중**: 각 구성요소의 역할이 분리되어 있기 때문에 각 역할에 집중할 수 있습니다. 이로 인해 코드의 품질과 유지 보수성이 향상됩니다.


## 3. MVVM패턴 -> 클린 아키텍처(MVVM 기반)

![](https://i.imgur.com/xOjIjnd.png)

- **Model**: 실제 데이터 및 비즈니스 로직을 포함합니다. 데이터베이스 액세스, 웹 서비스 호출 등의 작업을 수행합니다.
- **View**: 사용자에게 보여지는 UI 요소. 버튼, 텍스트박스, 라벨 등의 위젯 또는 컴포넌트를 포함합니다. **View는 ViewModel에 직접적으로 연결되어 데이터를 가져옵니다.**
- **ViewModel**: Model과 View 사이의 중개자 역할을 합니다. Model에서 데이터를 가져오고 View에 표시할 준비를 담당합니다. 또한 View에서 이벤트를 수신하고 그에 따라 Model을 업데이트합니다.


### **MVVM을 사용하는 이유**

1. **관심사의 분리**
	프로그램을 서로 다른 영역으로 나누어 각 영역이 서로 다른 관심사를 다루도록 설계함으로써, 프로그램의 복잡성을 줄일 수 있습니다.

2. **테스트 용이성**
	뷰와 뷰모델 사이의 인터페이스를 통해 뷰모델의 테스트가 용이해집니다

3. **구조화된 프로젝트 구조**
	MVVM 패턴은 뷰, 뷰모델 및 모델로 구성되며 각각의 구성 요소가 서로 분리되어 개발이 가능합니다.

4. **UI의 병렬 개발**
	MVVM 패턴을 사용하면 뷰와 뷰모델을 분리함으로써 UI의 개발과 동시에 뷰모델을 병렬적으로 개발할 수 있습니다.

5. **뷰의 추상화**
	뷰를 추상화한다는 말은 뷰에서 직접적으로 데이터를 처리하지 않고, 뷰모델(viewModel)을 통해 데이터를 주고받는 것을 말합니다.즉, 뷰모델은 UI와 비즈니스 로직 사이에서 중간 매개체 역할을 하여 UI와 비즈니스 로직 사이의 결합도를 낮춥니다.


## 3. MV패턴으로 코드 작성하기

```dart
import 'package:flutter/material.dart';  
  
// MV패턴으로 코드 작성해보기  
void main(){  
  runApp(MyApp());  
}  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      home: HomeView()  
    );  
  }  
}  
  
class HomeView extends StatefulWidget {  
  const HomeView({super.key});  
  
  @override  
  State<HomeView> createState() => _HomeViewState();  
}  
  
class _HomeViewState extends State<HomeView> {  
  // view 는 viewModel 주소 값을 가지고 있어야 한다.  
  final CounterViewModel viewModel = CounterViewModel();  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      appBar: AppBar(title: Text('viewModel 없이 작성'),),  
      body: Center(  
        child: Column(  
          mainAxisAlignment: MainAxisAlignment.center,  
          children: [  
            Text('간단한 뷰와 모델 예제'),  
            Text('${viewModel._count}', style: TextStyle(fontSize: 30),),  
            ElevatedButton(onPressed: (){  
              setState(() {  
                viewModel.incrementCounter();  
              });  
            }, child: Text('증가'))  
          ],  
        ),  
      ),  
    );  
  }  
}  
  
// viewModel 클래스 만들어 보기  
class CounterViewModel{  
  
  int _count = 0;  
  int get count => _count;  
  
  void incrementCounter() {  
    _count++;  
  }  
  
}
```


![](https://i.imgur.com/9WPA7tM.gif)



## 💡 Model과 ViewModel 차이점

1. **Model**:
    - **데이터와 관련된 로직**: **`Model`**은 애플리케이션의 비즈니스 로직, 유효성 검사, 데이터 연산, 데이터베이스 연산, 네트워크 연산 등을 처리합니다.
    - **상태 변경 없음**: **`Model`**은 사용자 인터페이스나 표현과 관련된 상태나 정보를 저장하거나 변경하지 않습니다. 순수하게 데이터와 관련된 로직만을 담당합니다.
2. **ViewModel**:
    - **뷰와 연관된 로직**: **`ViewModel`**은 특정 **`View`**와 연관된 로직을 담당합니다. 사용자의 입력을 처리하고, 해당 정보를 **`Model`**에 전달하여 데이터를 변경하거나 가져옵니다.
    - **뷰의 상태**: **`ViewModel`**은 **`View`**의 상태와 데이터를 관리합니다. 예를 들어, 로딩 스피너의 표시 여부, 에러 메시지의 표시 여부, 버튼의 활성화 상태 등의 UI 관련 상태를 저장하고 관리할 수 있습니다.
    - **뷰와의 직접적 연결 없음**: **`ViewModel`**은 **`View`**에 직접적으로 바인딩되지 않습니다. 이로 인해 **`ViewModel`**은 재사용성이 높아집니다. 특정 **`View`**와 직접적으로 연결되지 않기 때문에, 테스트하기도 용이합니다.
    - **데이터 변환**: **`ViewModel`**은 **`Model`**로부터 데이터를 가져와 **`View`**에서 표시하기 적절한 형식으로 변환하는 역할을 수행하기도 합니다. 예를 들어, **`Model`**에서는 날짜를 **`DateTime`** 형식으로 관리하지만, **`ViewModel`**은 이를 문자열 형식으로 변환하여 **`View`**에 제공할 수 있습니다.

요약하면, **`Model`**은 애플리케이션의 핵심 비즈니스 로직과 데이터 관리를 담당하며, **`ViewModel`**은 **`View`**와 연관된 로직 및 상태 관리를 담당합니다.

