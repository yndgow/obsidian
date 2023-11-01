
### 1. 구조 잡기

![](https://i.imgur.com/kPKC56A.png)


### 2. 코드

- models / todo

```dart
// 1. 데이터 만들기 - todo 클래스 설계
class Todo{
  bool isCompleted;
  String description;
  Todo(this.isCompleted, this.description);
}
```

- view_models / todo_view_model

```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../models/todo.dart';

class TodoViewModel extends StateNotifier<Todo>{
  // 데이터 + 행위
  //final Todo todo;

  // TodoViewModel({required this.todo});
  TodoViewModel() : super(Todo(false, 'MVVM학습'));

  // 행위
  void toggleCompleted() {
    // StateNotifier -> state 를 멤버로 가지고 있다.
    state = Todo(!state.isCompleted, state.description);
    // 상태를 변경하려면 깊은 복사 처리 해줘야 한다.
    // 얕은 복사, 깊은 복사
    // state.description = "a";
    // state.isCompleted = false;
  }
}

// 2. Provider 생성
// 최초 생성시에 상태 값을 가지게 됩니다. 해당 상태 값을 View에 연결해두면 , View는 해당 상태값을 통해 그림을 그립니다.
// 하지만 현재 학습하고 있는 Provider는 이후에 상태값이 변경이 되더라도 상태는 변경되지만 그림은 다시 그려주지 않습니다.
// final todoProvider = Provider<Todo>((ref) {
//   return Todo(true, "공부하기");
// });
// 상태는 변경 가능하지만 렌더링을 다시 시킬수 없는 창고 관리자.
// final todoViewModelProvider = Provider<TodoViewModel>((ref) {
//   return TodoViewModel(todo: Todo(false, "MVVM 학습"));
// });

// 상태 변경 및 UI를 다시 빌드하려면 StateNotifierProvider 를 사용해야한다.
// StateNotifierProvider 를 -> (전제조건) 객체에 확장을 StateNotifier 사용해야한다.

final todoViewModelProvider = StateNotifierProvider<TodoViewModel, Todo>((ref){
  return TodoViewModel();
});
```


- views / todo_page

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../models/todo.dart';
import '../view_models/todo_view_model.dart';

// 4. 소비자 위젯 선언하기
class TodoPage extends ConsumerWidget {
  const TodoPage({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 데이터 -> viewModel 로 분리
    // 화면에서는 viewModel만 바라보자
    // final viewModel = ref.read(todoViewModelProvider);
    final todo = ref.watch(todoViewModelProvider);


    return Center(
      child: InkWell(
        onTap: (){
          // 변수에 접근해서 처리 중
          //ref.read(todoViewModelProvider.notifier).state = Todo(false, '운동하기');
          ref.read(todoViewModelProvider.notifier).toggleCompleted();
        },
          child: Text(
            '해야 할 일 : ${todo.isCompleted}, ${todo.description}',
            style: TextStyle(fontSize: 25),)),
    );
  }
}
```

- main

```dart
import 'package:flutter/material.dart';  
import 'package:flutter_riverpod/flutter_riverpod.dart';  
import 'package:flutter_riverpod_v1/views/todo_page.dart';  
  
// 3. 범위 지정해주기 - 앱 루트 위젯트리부터 구역을 지정합니다.  
void main() {  
  runApp(ProviderScope(child: MyApp()));  
}  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      home: Scaffold(  
        body: TodoPage(),  
      ),  
    );  
  }  
}
```

![](https://i.imgur.com/l27Xlkc.gif)
