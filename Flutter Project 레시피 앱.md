
# 레시피 앱 만들어보기

![](https://i.imgur.com/gPrD0cm.png)



## 1. 구조 잡기

1. 프로젝트 생성

![](https://i.imgur.com/4yegJAP.png)


2. 패키지, 폴더 생성
	1. 기본 폴더, 패키지 생성
		![](https://i.imgur.com/FZqQsml.png)
		
	2. 하위 폴더 생성
	![](https://i.imgur.com/hVD7oFj.png)
	3. font 다운로드
	[PatuaOne Font](https://fonts.google.com/specimen/Patua+One)
	4. 이미지 다운로드
	![햄버거](https://i.imgur.com/08QntoO.jpg)
	![커피](https://i.imgur.com/J346gPH.jpg)
	![피자](https://i.imgur.com/0eMQM1s.jpg)
	5. 이미지, 폰트 넣기
	
		![](https://i.imgur.com/OUGYF4M.png)
	6. pubspec.yaml 변경

		![](https://i.imgur.com/3YDzn5p.png)
		![](https://i.imgur.com/a87kne3.png)
		![](https://i.imgur.com/CUf0pfI.png)
		family : 사용할 이름(아래의 폰트를 지정한 이름을 사용하겠다)
		![](https://i.imgur.com/ulxRbun.png)


## 2. 코드

![](https://i.imgur.com/5wxc1yJ.png)

- main.dart
```dart
import 'package:flutter/material.dart';  
import 'package:recipe_flutter/pages/recipe_page.dart';  
  
void main() {  
  runApp(MyApp());  
}  
class MyApp extends StatelessWidget {  
  // 생성자  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      debugShowCheckedModeBanner: false,  
      theme: ThemeData(fontFamily: 'PatuaOne'),  
      home: RecipePage(),  
    );  
  }  
}
```

- recipe_list_item.dart
```dart
import 'package:flutter/material.dart';  
  
class RecipeListItem extends StatelessWidget {  
  final String imageName;  
  final String title;  
  // 이미지 경로  
  // 타이틀  
  
  const RecipeListItem(this.imageName, this.title, {super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return Padding(  
      padding: const EdgeInsets.symmetric(vertical: 20),  
      child: Column(  
        crossAxisAlignment: CrossAxisAlignment.start,  
        children: [  
          Image.asset("assets/images/$imageName.jpeg", fit: BoxFit.cover,),  
          SizedBox(height: 10,),  
          Text(title, style: TextStyle(fontSize: 20),),  
          Text('aaaaaaaa', style: TextStyle(fontSize: 12, color: Colors.grey),),  
        ],  
      ),  
    );  
  }  
}
```

- recipe_menu.dart
```dart
import 'package:flutter/material.dart';  
  
class RecipeMenu extends StatelessWidget {  
  const RecipeMenu({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return Padding(  
      padding: const EdgeInsets.only(top: 8.0),  
      child: Row(  
        mainAxisAlignment: MainAxisAlignment.spaceAround,  
        children: [  
          _buildMenuItem(Icons.food_bank, "ALL"),  
          _buildMenuItem(Icons.emoji_food_beverage, "COFFEE"),  
          _buildMenuItem(Icons.fastfood, "Burger"),  
          _buildMenuItem(Icons.local_pizza, "Pizza"),  
        ],  
      ),  
    );  
  }  
}  
  
Widget _buildMenuItem(IconData mIcon, String text){  
  
  return Container(  
    width: 60,  
    height: 80,  
    decoration: BoxDecoration(  
      borderRadius: BorderRadius.circular(30),  
      border: Border.all(color: Colors.black12),  
    ),  
    child: Column(  
      mainAxisAlignment: MainAxisAlignment.center,  
      children: [  
        Icon(mIcon, color: Colors.redAccent),  
        SizedBox(height: 5,),  
        Text(text, style: TextStyle(color: Colors.black87),),  
      ],  
    ),  
  );  
}
```

- recipe_title.dart
```dart
import 'package:flutter/material.dart';  
  
class RecipeMenu extends StatelessWidget {  
  const RecipeMenu({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return Padding(  
      padding: const EdgeInsets.only(top: 8.0),  
      child: Row(  
        mainAxisAlignment: MainAxisAlignment.spaceAround,  
        children: [  
          _buildMenuItem(Icons.food_bank, "ALL"),  
          _buildMenuItem(Icons.emoji_food_beverage, "COFFEE"),  
          _buildMenuItem(Icons.fastfood, "Burger"),  
          _buildMenuItem(Icons.local_pizza, "Pizza"),  
        ],  
      ),  
    );  
  }  
}  
  
Widget _buildMenuItem(IconData mIcon, String text){  
  
  return Container(  
    width: 60,  
    height: 80,  
    decoration: BoxDecoration(  
      borderRadius: BorderRadius.circular(30),  
      border: Border.all(color: Colors.black12),  
    ),  
    child: Column(  
      mainAxisAlignment: MainAxisAlignment.center,  
      children: [  
        Icon(mIcon, color: Colors.redAccent),  
        SizedBox(height: 5,),  
        Text(text, style: TextStyle(color: Colors.black87),),  
      ],  
    ),  
  );  
}
```

- recipe_page.dart
```dart
import 'package:flutter/cupertino.dart';  
import 'package:flutter/material.dart';  
import 'package:recipe_flutter/components/recipe_list_item.dart';  
import 'package:recipe_flutter/components/recipe_menu.dart';  
import 'package:recipe_flutter/components/recipe_title.dart';  
  
class RecipePage extends StatelessWidget {  
  const RecipePage({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      backgroundColor: Colors.white,  
      appBar: _buildRecipeAppBar(),  
      body: Padding(  
        padding: const EdgeInsets.symmetric(horizontal: 20),  
        child: ListView(  
          //  주축 방향 - 세로  
          children: [  
            RecipeTitle(),  
            RecipeMenu(),  
            RecipeListItem("coffee", "Made Coffee"),  
            RecipeListItem("buger", "Made Buger"),  
            RecipeListItem("pizza", "Made Pizza"),  
          ],  
        ),  
      ),  
    );  
  }  
}  
  
// 함수 만들어보기  
AppBar _buildRecipeAppBar(){  
  return AppBar(  
    backgroundColor: Colors.white, // appbar 배경색 지정  
    elevation: 1.0,  
    actions: [  
      Icon(CupertinoIcons.search, color: Colors.black,),  
      SizedBox(width: 15,),  
      Icon(CupertinoIcons.heart, color: Colors.red,),  
      SizedBox(width: 15,),  
    ],  
  );  
}
```