
## 1. 폴더구조잡기


![](https://i.imgur.com/DBbJ53K.png)


## 2. 코드


- models - todo_item

```dart
// Model  
class TodoItem{  
  String title;  
  bool isDone;  
  
  TodoItem({required this.title, required this.isDone});  
  
  // TodoItem 비지니스 로직 생성 가능  
  
}
```

- view - todo_list_view

```dart
import 'package:class_todo_v1/view_models/todo_list_view_model.dart';  
import 'package:flutter/material.dart';  
  
class TodoListView extends StatefulWidget {  
  const TodoListView({super.key});  
  
  @override  
  State<TodoListView> createState() => _TodoListViewState();  
}  
  
class _TodoListViewState extends State<TodoListView> {  
  
  // view 는 viewModel만 바라보면 된다.  
  final TodoListViewModel listViewModel = TodoListViewModel();  
  
  // 사용자의 입력을 관리하기 위한 텍스트 컨트롤러  
  final TextEditingController _textEditingController = TextEditingController();  
  
  @override  
  Widget build(BuildContext context) {  
    return Column(  
      children: [  
        TextField(  
          controller: _textEditingController, // 컨트롤러 연결  
          decoration: InputDecoration( // 꾸미기  
            hintText: 'Enter todo item...',  
            suffix: IconButton( // 버튼 뒤에 붙이기  
              onPressed: () {  
                // 추후 로직 추가  
                setState(() {  
                  listViewModel.addItem(_textEditingController.text);  
                });  
                _textEditingController.clear();  
  
              }, icon: Icon(Icons.add),  
            )  
          ),  
        ),  
        Expanded(  
          child: ListView.builder(  
            itemCount: listViewModel.items.length,  
            itemBuilder: (context, index){  
            var item = listViewModel.items[index];  
            return ListTile(  
              title: Text(item.title),  
              trailing: Checkbox(  
                value: item.isDone,  
                onChanged: (value) {  
                  setState(() {  
                    listViewModel.toggleItem(item);  
                  });  
                },  
              ),  
            );  
          },),  
        ),  
      ],  
    );  
  }  
}
```

- view_models - todo_list_view_model

```dart
import '../models/todo_item.dart';  
  
class TodoListViewModel{  
  List<TodoItem> _items = [TodoItem(title: 'view만들기', isDone: true)];  
  
  // getter  
  List<TodoItem> get items => _items;  
  
  
  void addItem(String title){  
    _items.add(TodoItem(title: title, isDone: false));  
  }  
  
  // 넘겨 받은 todoItem 객체에 idDone 변수 값 변경하기  
  void toggleItem(TodoItem todo){  
    todo.isDone = !todo.isDone;  
  }  
}
```

- main

```dart
import 'package:class_todo_v1/view/todo_list_view.dart';  
import 'package:flutter/material.dart';  
  
void main() {  
  runApp(TodoApp());  
}  
  
class TodoApp extends StatelessWidget {  
  const TodoApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      debugShowCheckedModeBanner: false,  
      home: SafeArea(  
        child: Scaffold(  
          appBar: AppBar(title: const Text('Todo List'),),  
          body: TodoListView(),  
        ),  
      ),  
    );  
  }  
}
```



## 3. 실행화면


![](https://i.imgur.com/eRCWEfT.gif)


![](https://i.imgur.com/CCyT1Kg.gif)
