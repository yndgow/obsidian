
## 1. 구조잡기

![](https://i.imgur.com/RFFYT7O.png)

![](https://i.imgur.com/EtuAAnO.png)

![](https://i.imgur.com/rTpcd2L.png)


![WpvZ3sF.png](https://i.imgur.com/WpvZ3sF.png)
![](https://i.imgur.com/WpvZ3sF.png)

## 2. 코드
### src/main/java

- advice / GlobalExceptionAdvice

```java
package com.tencoding.todo.advice;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestControllerAdvice
public class GlobalExceptionAdvice {

	@ExceptionHandler(Exception.class)
	public void exception(Exception e) {
		log.error("-------------------------");
		log.error(e.getClass().getName());
		log.error(e.getMessage());
	}
	
	@ExceptionHandler(SqlException.class)
	public ResponseEntity<String> sqlError(SqlException e) {	
		log.error(e.toString());
		return new ResponseEntity<>("ashdflkhasdlkfhaslkdhfashdf", e.getStatus());
	}
}
```

- advice / SqlException

```java
package com.tencoding.todo.advice;

import org.springframework.http.HttpStatus;
import lombok.Data;

@Data
public class SqlException extends RuntimeException {

	private HttpStatus status; 
	
	public SqlException(String message, HttpStatus status) {
		super(message);
		this.status = status;
	}
}
```

- controller / TodoController

```java
package com.tencoding.todo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.tencoding.todo.dto.ResponseDTO;
import com.tencoding.todo.dto.TodoDTO;
import com.tencoding.todo.repository.entity.TodoEntity;
import com.tencoding.todo.service.TodoService;

@RestController
@RequestMapping("/todos")
public class TodoController {

	private final TodoService todoService;
	
	@Autowired // 명시적 사용
	public TodoController(TodoService todoService) {
		this.todoService = todoService;
	}

	// http://localhost:80/todos/all
	@GetMapping("/all")
	public ResponseEntity<List<TodoEntity>> getTodoList() {
		// TODO - SQL 오류 발생 시켜 놓았음 !!!
		List<TodoEntity> todos = todoService.readAllTodo();
		return new ResponseEntity<>(todos, HttpStatus.OK);
	}

	@GetMapping("/{id}")
	public ResponseDTO<?> getTodoById(@PathVariable Integer todoId) {
		TodoEntity todo = todoService.readTodById(todoId);
		
		ResponseDTO<TodoEntity> responseDTO = new ResponseDTO<>();
		responseDTO.setCode(1);
		responseDTO.setMessage("정상처리 되었습니다.");
		responseDTO.setData(todo);
		
		if (todo != null) {
			return responseDTO;
		} else {
			return responseDTO;
		}
	}

	@PostMapping("/create")
	public ResponseEntity<?> postTodo(@RequestBody TodoDTO todoDTO) {
		int result = todoService.createTodo(todoDTO);
		return new ResponseEntity<>(result, HttpStatus.CREATED);
	}

	@PutMapping("/update/{id}")
	public ResponseEntity<?> putTodoById(@PathVariable Integer id, @RequestBody TodoDTO dto) {
		int result = todoService.updateTodoById(id, dto);
		return new ResponseEntity<>(result, HttpStatus.OK);
	}

	@DeleteMapping("/delete/{id}")
	public ResponseEntity<?> deleteTodoById(@PathVariable Integer id) {
		int result = todoService.deleteTodoById(id);
		return new ResponseEntity<>(result, HttpStatus.OK);
	}
}
```

- controller / UserController

```java
package com.tencoding.todo.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.tencoding.todo.dto.UserDTO;
import com.tencoding.todo.repository.entity.UserEntity;
import com.tencoding.todo.service.UserService;
import com.tencoding.todo.utils.JwtUtil;

@RestController
@RequestMapping("/user")
public class UserController {
	
	private final UserService userService;
	// 코드 추가 
	private final JwtUtil jwtUtil;
	
	// 코드 수정
	@Autowired	
	public UserController(UserService userService, JwtUtil jwtUtil) {
		this.userService = userService;
		this.jwtUtil = jwtUtil;
	}
	
	// 주소 설계 
	// 회원 가입 요청 -- form, HTTP Message body
	@PostMapping("/sign-up")
	public ResponseEntity<?> signUp(@RequestBody UserDTO userDTO) {
		// 데이터 유효성 검사- 생략  
		int result = userService.signUp(userDTO); // 201 , 200, 404  
		return ResponseEntity.status(HttpStatus.CREATED).body(result);
	}  
	
	
	// 로그인 요청 --> 보안상 이유
	@PostMapping("/sign-in")
	public ResponseEntity<?> signin(@RequestBody UserDTO userDTO) {

		UserEntity user = userService.signin(userDTO);
		// 세션 처리 ---> JWT
		if(user != null) {
			String token = jwtUtil.generateToken(user.getEmail(), user.getUserId());
			// 헤더 셋팅
			HttpHeaders httpHeaders = new HttpHeaders();
			// JWT 헤더는 약속 Bearer , 으로 반드시 시작해야 한다.
			httpHeaders.add("Authorization", "Bearer " + token);
			return ResponseEntity.ok().headers(httpHeaders).body(user);
		} else {
			return new ResponseEntity<>("로그인 실패", HttpStatus.UNAUTHORIZED);
		}
	}
	
	// TEST
	// http://localhost:80/user/token-test
	@GetMapping("/token-test")
	public String testToken() {
		// eyJhbGciOiJIUzUxMiJ9.eyJ1c2VyRW1haWwiOiJ0ZXN0QG5hdmVyLmNvbSIsInVzZXJJZCI6MTAsImV4cCI6MTY5ODQ3NDAxNn0.3SGGdFMME3FCVDNrFaQLOlqzN-i28khCAnG65PJe49BLdm2NdHK5AckXj-3AqQAV-Y10idjhdRiIOz2PR-OrMg
		return jwtUtil.generateToken("test@naver.com", 10);
	}
}
```

제네릭으로 응답하기

- dto / ResponseDTO

```java
package com.tencoding.todo.dto;

import lombok.Data;

@Data
public class ResponseDTO<T> {

	int code;
	String message;
	T data;
}
```

- dto / TodoDTO

```java
package com.tencoding.todo.dto;

import lombok.Data;

@Data
public class TodoDTO {
	private String title;
	private boolean completed;
	private int userId; 
}

```

- dto / UserDTO

```java
package com.tencoding.todo.dto;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class UserDTO {
	private String username; 
	private String email; 
	private String password;
}

```

- filter / FilterConfig
- filter / JwtRequestFilter
- repository / entity / TodoEntity
- repository / entity / UserEntity
- repository / mapper / TodoRepository
- repository / mapper / UserRepository
- service / TodoService
- service / UserService
- utils / JwtUtil

### src/main/resources

- mapper / todo.xml
- mapper / user.xml



