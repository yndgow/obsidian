# 플러터 layout 기본학습하기

[# Building layouts](https://docs.flutter.dev/ui/layout/tutorial)

### 1. 프로젝트 생성


![](https://i.imgur.com/l158Olh.png)


- 시안

![](https://i.imgur.com/WvGwuiC.jpg)



### 2. 폴더 구조 잡기


![](https://i.imgur.com/i6IvZKO.png)

- pubspec.yaml (이미지 설정)

```yaml
assets:  
  - assets/lake.jpg
```

### 3. 코드 내용

- main.dart

```dart
import 'package:flutter/material.dart';  
import 'package:flutter_layout_01/components/favorited_widte.dart';  
  
void main() => runApp(Myapp());  
  
class Myapp extends StatelessWidget {  
  const Myapp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    Widget textSection = Container(  
      padding: const EdgeInsets.all(32),  
      child: const Text(  
        'Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese '  
        'Alps. Situated 1,578 meters above sea level, it is one of the '        
        'larger Alpine Lakes. A gondola ride from Kandersteg, followed by a '        
        'half-hour walk through pastures and pine forest, leads you to the '        
        'lake, which warms to 20 degrees Celsius in the summer. Activities '        
        'enjoyed here include rowing, and riding the summer toboggan run.',  
        softWrap: true,  
      ),  
    );  
  
    // theme 속성을 가져오기  
    // 위젯 트리 내의 현재 위치에 대한 참조 및 핸들링 처리  
    // 위젯 트리에 다양한 메타 데이터나 부모 위젯에 접근하여  
    // 상호작용할 수 있도록 하는 BuildContext 객체  
    Color color = Theme.of(context).primaryColor;  
    Widget buttonSection = Row(  
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,  
      children: [  
        _buildButtonColumn(color, Icons.call, 'CALL'),  
        _buildButtonColumn(color, Icons.near_me, 'ROUTE'),  
        _buildButtonColumn(color, Icons.share, 'SHARE'),  
      ],  
    );  
  
    // 지역 변수 + 연산  
    Widget titleSection = Container(  
      padding: const EdgeInsets.all(32),  
      child: Row(  
        children: [  
          Expanded(  
            child: Column(  
              crossAxisAlignment: CrossAxisAlignment.start,  
              children: [  
                Container(  
                  padding: const EdgeInsets.only(bottom: 8),  
                  child: const Text(  
                    'Oeschinen Lake Campground',  
                    style: TextStyle(  
                        fontWeight: FontWeight.bold),  
                  ),  
                ),  
                Text(  
                  'Kandersteg, Switzerland',  
                  style: TextStyle(  
                    color: Colors.grey[500],  
                  ),  
                ),  
              ],  
            ),  
          ),  
          FavoriteWidget(),  
        ],  
      ),  
    );  
  
    return MaterialApp(  
      title: 'Flutter Layout demo',  
      home: Scaffold(  
        appBar: AppBar(title: const Text('layout demo'),),  
        body: ListView(  
          children: [  
            Image.asset(  
              'assets/lake.jpg',  
              width: 600,  
              height: 240,  
              fit: BoxFit.cover,  
            ),  
            titleSection,  
            buttonSection,  
            textSection,  
          ],  
  
        )  
      ),  
    );  
  }  
}  
  
Column _buildButtonColumn(Color color, IconData icon, String label){  
  return Column(  
    children: [  
      Icon(icon, color: color,),  
      Container(  
        margin: const EdgeInsets.only(top: 8),  
        child: Text(  
          label,  
          style: TextStyle(  
              fontSize: 12,  
              fontWeight: FontWeight.w600,  
              color: color  
          ),  
        ),  
      ),  
    ],  
  );  
}
```

- components / favorited_widte.dart

```dart
import 'package:flutter/material.dart';  
  
class FavoriteWidget extends StatefulWidget {  
  const FavoriteWidget({super.key});  
  
  @override  
  State<FavoriteWidget> createState() => _FavoriteWidgetState();  
}  
  
class _FavoriteWidgetState extends State<FavoriteWidget> {  
    
  // 1. 변수 선언 해보기  
  bool _isFavorited = true;  
  int _favoritedCount = 41;  
  
  @override  
  Widget build(BuildContext context) {  
    return Row(  
      children: [  
        Container(  
          padding: EdgeInsets.all(4.0),  
          child: IconButton(  
            icon: _isFavorited  
                ? const Icon(Icons.star , color: Colors.red,)  
                : const Icon(Icons.star_border, color: Colors.red,),  
            onPressed: (){  
              setState(() {  
                if(_isFavorited){  
                  _isFavorited = false;  
                  _favoritedCount -= 1;  
                }else{  
                  _isFavorited = true;  
                  _favoritedCount += 1;  
                }  
              });  
            },  
          ),  
        ),  
        SizedBox(  
          width: 40,  
          child: Text('$_favoritedCount'),  
        ),  
      ],  
    );  
  }  
}
```


### 4. 결과화면


![](https://i.imgur.com/ttkE6hM.png)


![](https://i.imgur.com/4LpsllL.gif)
