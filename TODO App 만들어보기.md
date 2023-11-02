### 1. 구조잡기

![](https://i.imgur.com/BAT46Wc.png)



### 2. 코드

- models / dto / user_dto

```dart
class UserDto {  
  
  String? username;  
  String email;  
  String password;  
  
  UserDto({required this.email, required this.password, this.username});  
  
  Map<String, dynamic> toJson(){  
    return {  
      'email' : email,  
      'password' : password,  
      'username' : username ?? ''  
    };  
  }  
  
  @override  
  String toString() {  
    return 'UserDto{username: $username, email: $email, password: $password}';  
  }  
}

```

- models / entity / user_entity

```dart
class UserEntity{

  int userId;
  String username;
  String email;

  UserEntity({required this.userId, required this.username, required this.email});

  @override
  String toString() {
    return 'UserEntity{userId: $userId, username: $username, email: $email}';
  }

  // 변환 기능 추가
  // factory 메서드 활용
  factory UserEntity.fromJson(Map<String, dynamic> json){
    return UserEntity(
        userId: json['userId'] as int,
        username: json['username'] as String,
        email: json['email'] as String
    );
  }
}
```

- repository / remote / user_repository

```dart
import 'package:class_my_todo_v1/models/entity/user_entity.dart';  
  
import '../../models/dto/user_dto.dart';  
  
abstract class UserRepository {  
  Future<int> signUp(UserDto userDto);  
  Future<UserEntity> signIn(UserDto userDto);  
}
```

- repository / remote / user_repository_impl

```dart
import 'package:class_my_todo_v1/models/dto/user_dto.dart';  
import 'package:class_my_todo_v1/models/entity/user_entity.dart';  
import 'package:class_my_todo_v1/repository/remote/user_repository.dart';  
import 'package:class_my_todo_v1/shared/api.dart';  
import 'package:class_my_todo_v1/shared/http.dart';  
import 'package:dio/dio.dart';  
  
// 싱글톤 처리 - 계속 new 하지 않기 위해  
class UserRepositoryImpl implements UserRepository{  
  
  // 명명된 생성자 single 일반적 사용  
  UserRepositoryImpl._single();  
  
  // 외부에서 접근할 수 있는 변수를 static 처리  
  static final UserRepositoryImpl _intance = UserRepositoryImpl._single();  
  
  // 1 dart 언어에서 factory 키워드 - 생성자, 메서드  
  // factory 생성자 - 매번 클래스의 인스턴스를 같은 녀석으로 반환 처리  
  factory UserRepositoryImpl() {  
    return _intance;  
  }  
  
  
  @override  
  Future<UserEntity> signIn(UserDto userDto) async {  
    // 사용자의 정보 요청 -> GET(보안상의 이유로 POST 처리)  
    // BaseURI + epSignIn    // 'http://192.168.0.45:80/sign-in';  
    // userDto --> json 문자열 형식  
    // dio -> 이 처리를 하기 위해서 규칙 --> DTO 안에 json으로 바꿔줘야한다.  
    try{  
      Response response = await dio.post(epSignIn, data: userDto);  
      print(userDto);  
      print(response.headers['Authorization']); // token 확인  
      print(response.data); // body 영역 데이터 확인  
      print(response.toString()); // response 전체 확인  
      return UserEntity.fromJson(response.data);  
    }catch(e){  
      print(e.toString());  
      return UserEntity(userId: -1, username: '', email: '');  
    }  
  }  
  
  @override  
  Future<int> signUp(UserDto userDto) {  
    // TODO: implement signUp  
    throw UnimplementedError();  
  }  
}
```

- shared / api

```dart
// basURI -> 서버 주소 + 포트
// endPoint -> 미리 정의
// 자신의 IP 주소 입력해야함 - 192.168.0.45

const baseUri = 'http://192.168.0.45:80';
const pathUser = '/user';
const epSignUp = '$pathUser/sign-up';
const epSignIn = '$pathUser/sign-in';

const pathTodo = '/todos';
const epTodoList = '$pathTodo/all';
const epTodoById = '$pathTodo/{id}';
const epTodoCreate = '$pathTodo/create';

```

- shared / constant

```dart
// 하드 코딩할 문자들
```

- shared / http

```dart
// HTTP 통신 객체 설정  
import 'package:class_my_todo_v1/shared/api.dart';  
import 'package:dio/dio.dart';  
  
final dio = Dio(  
    BaseOptions(  
      baseUrl: baseUri,  
      contentType: 'application/json; charset=utf-8',  
));
```

- test / user_repository_impl_test

```dart
// 단위 테스트 작성해보기  
import 'package:class_my_todo_v1/models/dto/user_dto.dart';  
import 'package:class_my_todo_v1/models/entity/user_entity.dart';  
import 'package:class_my_todo_v1/repository/remote/user_repository.dart';  
import 'package:class_my_todo_v1/repository/remote/user_repository_impl.dart';  
import 'package:dio/dio.dart';  
import 'package:flutter_test/flutter_test.dart';  
  
void main(){  
  
  group('회원관리 도메인 테스트', () {  
  
    test('사용자 로그인 테스트', () async {  
      // given  
      UserRepository userRepository = UserRepositoryImpl();  
      UserDto userDto = UserDto(email: 'hong@example.com', password: 'hong1234');  
  
      // when  
      UserEntity userEntity = await userRepository.signIn(userDto);  
  
      // then  
      expect(userEntity, isA<UserEntity>()); // 데이터 타입이 UserEntity 확인  
      expect(userEntity.username, '홍길동');  
  
    });  
  });  
}
```


### 3. 테스트 결과

