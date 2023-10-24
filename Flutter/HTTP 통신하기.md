# Flutter HTTP


### pub.dev

https://pub.dev/packages/http

https://pub.dev/packages/dio


## 1. dio 라이브러리란

dio는 HTTP 클라이언트 라이브러리입니다. Flutter와 Dart 웹, CLI 및 서버 사이드 개발에서 사용됩니다.

- **풍부한 기능**: **`dio`**는 기본 HTTP 요청뿐만 아니라 FormData, File Upload/Download, Timeout 등 다양한 기능을 지원합니다.
- **가동성**: 쉽게 커스터마이징 가능한 요청과 응답 인터셉터, HTTP 요청 캔슬, Cookie 관리, Cache, 그리고 기타 많은 기능이 내장되어 있습니다.
- **효율성**: **`dio`**는 Dart의 비동기 특성을 최대한 활용하여 효율적인 네트워크 요청을 수행합니다.

```dart
dependencies:
  dio: ^5.3.3
```

## 2. dio 라이브러리 세팅하기

1. dio 라이브러리를 사용하면 http 통신을 쉽게 사용할 수 있습니다.
2. baseUrl 속성에는 자신의 IP주소를 입력합니다.
3. contentType 속성에는 요청시에 보낼 데이터 타입을 정의합니다. 우리가 연결할 서버는 JSON으로 응답과 요청을 처리합니다.
4. FlutterSecureStorage는 암호화된 형태로 데이터를 안전하게 저장하고 검색할 수 있는 플러터 플러그인입니다. 이 플러그인을 사용하여 인증 토큰과 같은 데이터를 안전하게 보호할 수 있습니다.

```dart
android:exported="true"
```


[테스트 통신 샘플 주소](https://jsonplaceholder.typicode.com/todos)


## 3. 코드 작성

```dart
import 'dart:convert';  
  
import 'package:dio/dio.dart';  
import 'package:flutter/material.dart';  
  
void main() {  
  runApp(MyApp());  
}  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      debugShowCheckedModeBanner: false,  
      title: 'Dio Example',  
      theme: ThemeData(  
        primarySwatch: Colors.orange  
      ),  
      home: SafeArea(  
        child: MyHomePage(),  
      ),  
    );  
  }  
}  
  
  
// 기본코드 작성 - 1class MyHomePage extends StatefulWidget {  
  const MyHomePage({super.key});  
  
  @override  
  State<MyHomePage> createState() => _MyHomePageState();  
}  
// sub class  
class _MyHomePageState extends State<MyHomePage> {  
  Dio _dio = Dio();  
  List<dynamic> _todos = [];  
  // 통신하기 위해 서버 필요  
  
  String url = 'https://jsonplaceholder.typicode.com/';  
  
  @override  
  void initState() {  
    super.initState();  
    _fetchTodos();  
  }  
  
  // 비동기 통신 -> Future (GET)  Future<void> _fetchTodos() async{  
    Response response = await _dio.get('https://jsonplaceholder.typicode.com/todos');  
    print(response.statusCode);  
    print('response.data : ${response.data}');  
    print('response.data type check : ${response.runtimeType}');  
    _todos = response.data;  
  }  
  
  // POST 함수 만들기  
  Future<void> _fetchTodoPost() async {  
    String newUrl = '${url}posts';  
    Response response = await _dio.post(newUrl, data: {'title':'foo', 'body':'bar', 'userId':2});  
    print(response.statusCode);  
    print(response.data);  
  }  
  
  // PUT 함수 만들기  
  Future<void> _fetchTodoPut() async {  
    String newUrl = '${url}posts/2';  
    Response response = await _dio.put(newUrl, data: {'title':'foo2', 'body':'bar2', 'userId':2});  
    print(response.statusCode);  
    print(response.data);  
  }  
  // Delete 함수 만들기  
  Future<void> _fetchTodoDelete(int id) async {  
    String newUrl = '${url}posts/3';  
    Response response = await _dio.delete(newUrl);  
    print(response.statusCode);  
    print(id);  
  }  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      body: ListView.builder(  
        itemBuilder: (context, index){  
          return ListTile(  
            leading: Text('${index + 1}'),  
            title: Text('title : ${_todos[index]['title']}'),  
            subtitle: Row(  
              mainAxisAlignment: MainAxisAlignment.spaceBetween,  
              children: [  
                ElevatedButton(onPressed: (){  
                  _fetchTodoPost();  
                }, child: Text('post')),  
                ElevatedButton(onPressed: (){  
                  _fetchTodoPut();  
                }, child: Text('put')),  
                ElevatedButton(onPressed: (){  
                  _fetchTodoDelete(_todos[index]['id']);  
                }, child: Text('delete')),  
              ],  
            ),  
            trailing: IconButton(  
              onPressed: () {  
                print('상세보기 화면 이동');  
              },  
              icon: Icon(Icons.add),  
            ),  
          );  
        },  
        itemCount: _todos.length,  
      ),  
    );  
  }  
}
```

![](https://i.imgur.com/me4Rm4y.png)


- post

```
I/flutter (13385): 201
I/flutter (13385): {title: foo, body: bar, userId: 2, id: 101}
```

- put

```
I/flutter (13385): 200
I/flutter (13385): {title: foo2, body: bar2, userId: 2, id: 2}
```

- delete

```
I/flutter (13385): 200
I/flutter (13385): 1
```

- +버튼
```
I/flutter (13385): 상세보기 화면 이동
```
