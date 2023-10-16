# BuildContext 와 위젯 키

## 1. BuildContext 란

BuildContext(context) 는 위젯 트리의 현재 위치에 대한 정보를 가진 객체입니다. 이 context를 통해 위젯 트리에서 상위 위젯을 참조하거나 다양한 메타데이터(예: 테마, MediaQuery, Locale 등)를 얻을 수 있습니다. 단, context를 통해서 위젯 트리의 "상위" 위젯만 참조할 수 있습니다. 자식 위젯이나 형제 위젯을 직접적으로 참조하는 것은 context를 통해서는 불가능합니다. 그리고 context는 위젯 자체와 연결되어 있지 않습니다. 즉, BuildContext 객체는 위젯이 다시 빌드될 때마다 새롭게 생성될 수 있습니다.

## 2. 위젯 키란 뭘까? - Widget({Key? key})

위젯 키는 Flutter에서 위젯의 고유 식별자로 사용됩니다. 그래서 모든 위젯은 키 값을 가질 수 있습니다. Flutter는 위젯의 형태와 위치를 기반으로 위젯을 재사용하거나 다시 만듭니다. 그러나 특정 상황에서 개발자는 Flutter에게 어떤 위젯이 어떤 위젯과 일치하는지 명확하게 알려줄 필요가 있습니다. 이런 경우에 Key를 활용 할 수 있습니다.


![](https://i.imgur.com/ekJ1uhq.png)


시나리오 코드 - 1 (위젯키가 필요 없는 경우)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  List<Widget> widgetList = [A(), B()];

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: SafeArea(
        child: Column(
          children: [
            Row(
              children: widgetList,
            ),
            ElevatedButton(
              onPressed: () {},
              child: Text('toggle'),
            )
          ],
        ),
      ),
    );
  }
}

//////////////////////////////////////////

class A extends StatefulWidget {
  const A({super.key});

  @override
  State<A> createState() => _AState();
}

class _AState extends State<A> {
  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Container(
        height: 300,
        color: Colors.orange[300],
        child: Center(
          child: Text("A"),
        ),
      ),
    );
  }
}

//////////////////////////////////////////

class B extends StatefulWidget {
  const B({super.key});

  @override
  State<B> createState() => _BState();
}

class _BState extends State<B> {
  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Container(
        height: 300,
        child: Center(
          child: Text("B"),
        ),
      ),
    );
  }
}
```


시나리오 코드 - 2 (위젯키가 필요없는 경우)
```dart
[import 'package:flutter/material.dart';  
  
void main(){  
  runApp(MyApp());  
}  
  
class MyApp extends StatefulWidget {  
  const MyApp({super.key});  
  
  @override  
  State<MyApp> createState() => _MyAppState();  
}  
  
class _MyAppState extends State<MyApp> {  
  
  List<Widget> widgetList = [  
    A(Colors.orange, key: UniqueKey()),  
    A(Colors.blue, key: UniqueKey(),)  
  ];  
  
  onChange(){  
    print('1111');  
    setState(() {  
      // A 위젯을 삭제하고 - widgetList.removeAt(0)      // 삭제하면 삭제한 위젯을 반환 까지함  
      widgetList.insert(1, widgetList.removeAt(0));  
    });  
  }  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      home: SafeArea(  
        child: Scaffold(  
          body: Column(  
            children: [  
              Row(  
                children: widgetList,  
              ),  
              ElevatedButton(  
                  onPressed: onChange,  
                  child: Text('toggle')  
              )  
            ],  
          ),  
        ),  
      ),  
    );  
  }  
}  
// /Colors.orange[300]  
/////////////////////////////////////////////////  
  
class A extends StatefulWidget {  
  Color color;  
  
  A(this.color, {super.key});  
  
  @override  
  State<A> createState() => _AState(this.color);  
}  
  
class _AState extends State<A> {  
  Color _color;  
  // 기본생성자에서 사용자 정의 생성자로 변경됨;  
  _AState(this._color);  
  
  @override  
  Widget build(BuildContext context) {  
    return Expanded(  
      child: Container(  
        height: 300,  
        color: _color,  
        child: Center(  
          child: Text('A'),  
        ),  
      ),  
    );  
  }  
}](<import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State%3CMyApp%3E createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  List<Widget> widgetList = [A(), B()];

  onChange() {
    setState(() {
      // A위젯을 삭제하고 - widgetList.removeAt(0)
      // 삭제하면 삭제한 위젯을 반환 까지 함
      widgetList.insert(1, widgetList.removeAt(0));
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: SafeArea(
        child: Scaffold(
          body: Column(
            children: [
              Row(
                children: widgetList,
              ),
              ElevatedButton(
                onPressed: onChange,
                child: Text('toggle'),
              )
            ],
          ),
        ),
      ),
    );
  }
}

//////////////////////////////////////////

class A extends StatefulWidget {
  const A({super.key});

  @override
  State<A> createState() => _AState();
}

class _AState extends State<A> {
  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Container(
        height: 300,
        color: Colors.orange[300],
        child: Center(
          child: Text("A"),
        ),
      ),
    );
  }
}

//////////////////////////////////////////

class B extends StatefulWidget {
  const B({super.key});

  @override
  State<B> createState() => _BState();
}

class _BState extends State<B> {
  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Container(
        color: Colors.blue[300],
        height: 300,
        child: Center(
          child: Text("B"),
        ),
      ),
    );
  }
}>)
```

![](https://i.imgur.com/X9f4mEH.png)


💡 change() 메서드를 실행하면 사실 위젯은 변경이 되지만 색상은 변경 되지 않습니다. 즉, 위젯 트리 구조가 변경되면서 Stateful 하위 객체인 State 객체를 각 위젯 객체에 올바르게 연결해야 하지만 데이터 타입이 같아서 제대로 식별하지 못하는 경우 입니다.







