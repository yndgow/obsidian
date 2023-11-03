
### 가입하기 기능 추가

1. 구조잡기

![](https://i.imgur.com/c4WbLhL.png)


2. 코드
- models / dto / todo_dto

```dart
class TodoDto {
  String title;
  bool completed;

  TodoDto(this.title, this.completed);

  // object --> http 통신 (문자열 --> json 형식)  ---> 서버(spring, php, .net)
  // object --> 문자 (dio 라이브러리) -> Map 구조 형식을 --> 문자열 변경(JSON)

  // 반드시 toJson() 메서드를 오버라이드 해주어야 한다. 필수 !!!
  Map<String, dynamic> toJson() {
    return {'title': title, 'completed': completed};
  }

  @override
  String toString() {
    return 'TodoDto{title: $title, completed: $completed}';
  }
}
```

- models / dto / todo_entity

```dart
class TodoEntity {
  int id;
  String title;
  String userId;
  bool completed;

  TodoEntity(
      {required this.id,
      required this.title,
      required this.userId,
      required this.completed});

  @override
  String toString() {
    return 'TodoEntity{id: $id, title: $title, userId: $userId, completed: $completed}';
  }
}
```

- repository / remote / todo_repository

```dart
import 'package:class_my_todo_v1/models/dto/todo_dto.dart';
import 'package:class_my_todo_v1/models/entity/todo_entity.dart';

abstract class TodoRepository {
  Future<List<TodoEntity>> todoList();
  Future<TodoEntity> findByIdTodo(int todoId);
  Future<int> createTodo(TodoDto todoDto); // 생성
  Future<int> updateTodoById(TodoDto todoDto); // 생성
  void deleteTodoById(int todoId);
}
```

- repository / remote / todo_repository_impl (파일만 생성)

```dart
import 'package:class_my_todo_v1/models/dto/todo_dto.dart';
import 'package:class_my_todo_v1/models/entity/todo_entity.dart';
import 'package:class_my_todo_v1/repository/remote/todo_repository.dart';

class TodoRepositoryImpl implements TodoRepository {
  @override
  Future<int> createTodo(TodoDto todoDto) {
    // TODO: implement createTodo
    throw UnimplementedError();
  }

  @override
  void deleteTodoById(int todoId) {
    // TODO: implement deleteTodoById
  }

  @override
  Future<TodoEntity> findByIdTodo(int todoId) {
    // TODO: implement findByIdTodo
    throw UnimplementedError();
  }

  @override
  Future<List<TodoEntity>> todoList() {
    // TODO: implement todoList
    throw UnimplementedError();
  }

  @override
  Future<int> updateTodoById(TodoDto todoDto) {
    // TODO: implement updateTodoById
    throw UnimplementedError();
  }
}
```