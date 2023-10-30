
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

```java
package com.tencoding.todo.filter;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import lombok.extern.slf4j.Slf4j;

// 어노테이션은 해당 클래스가 스프링 프레임워크의 설정 정보를 담는 클래스를 나타낸다.
// 이 Configuration @Component을 상속 받고 있다.
// 이 클래스에 내부에서 Bean 객체를 더 생성해야 할 때 사용

@Slf4j
@Configuration // - 하위에 빈을 더 처리 할때 : 설정 정보를 담고 있는 녀셕이다. 
public class FilterConfig {
	
	// 1. 우리가 정의한 JWT 관련된 필터 동작 객체를 생성자 의존 주입 받는다.
	@Autowired
	private JwtRequestFilter jwtRequestFilter;
	
	// 특정 메서들 만들어서 URI 패턴을 등록 처리
	@Bean
	public FilterRegistrationBean<JwtRequestFilter> loggingFilter() {
		log.error("스프링 부트 구동시 초기화 확인 - 1");
		FilterRegistrationBean<JwtRequestFilter> registrationBean = new FilterRegistrationBean<>();
		registrationBean.setFilter(jwtRequestFilter);
		registrationBean.addUrlPatterns("/todos/*");
		return registrationBean;
	}
}
```

- filter / JwtRequestFilter

```java
package com.tencoding.todo.filter;

import java.io.IOException;
import java.net.http.HttpResponse;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import com.tencoding.todo.utils.JwtUtil;

import lombok.extern.slf4j.Slf4j;



@Slf4j
@Component
public class JwtRequestFilter implements Filter {
	
	@Autowired
	private JwtUtil jwtUtil;
	// doFilter -> 약속 : 요청이 오면 반드시 doFilter가 호출 된다. 
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		
		log.info("JWT Filter 동작 확인 - 1");
		HttpServletRequest httpServletRequest = (HttpServletRequest)request;
		String authHader = httpServletRequest.getHeader("Authorization");
		
		if(authHader != null) {
			// 토큰 존재 여부 확인 
			if(!authHader.startsWith("Bearer ")) {
				sendError(response); 
				return; 
			}
			
			// 토큰 존재 여부 확인 
			if(!authHader.startsWith("Bearer ")) {
				sendError(response);
				return;
			}

			String jwtToken = authHader.substring(7);
			// 토큰 유효성 확인 
			if(!jwtUtil.validateToken(jwtToken)) {
				sendError(response); 
				return; 
			}
		}
		log.info("JWT Filter 동작 확인 - 2");
		chain.doFilter(request, response);
	}

	private void sendError(ServletResponse response) throws IOException {
		HttpServletResponse httpServletResponse = (HttpServletResponse)response;
		httpServletResponse.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Invalid Token");
	}
}
```

- repository / entity / TodoEntity

```java
package com.tencoding.todo.repository.entity;

import lombok.Data;

@Data
public class TodoEntity {

	private Integer id; 
	private Integer userId; 
	private String title;
	private boolean completed;
}

```

- repository / entity / UserEntity

```java
package com.tencoding.todo.repository.entity;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class UserEntity {
	private int userId; 
	private String username; 
	private String email; 
}
```

- repository / mapper / TodoRepository

```java
package com.tencoding.todo.repository.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

import com.tencoding.todo.dto.TodoDTO;
import com.tencoding.todo.repository.entity.TodoEntity;

@Mapper
public interface TodoRepository {

 public List<TodoEntity> findAllList();
 public int createTodo(TodoDTO todoDTO);
 public TodoEntity findByIdTodo(Integer todoId);
 public int updateById(@Param("todoId") Integer todoId, @Param("todoDTO") TodoDTO todoDTO);
 public int deleteById(Integer id);
}

```

- repository / mapper / UserRepository

```java
package com.tencoding.todo.repository.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.tencoding.todo.dto.UserDTO;
import com.tencoding.todo.repository.entity.UserEntity;

@Mapper
public interface UserRepository {

	// 회원 가입
	int signUp(UserDTO userDTO);
	// 로그인 
	UserEntity signIn(UserDTO userDTO);  
}
```

- service / TodoService

```java
package com.tencoding.todo.service;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Service;

import com.tencoding.todo.advice.SqlException;
import com.tencoding.todo.dto.TodoDTO;
import com.tencoding.todo.repository.entity.TodoEntity;
import com.tencoding.todo.repository.mapper.TodoRepository;

@Service
public class TodoService {

	private final TodoRepository todoRepository;

	public TodoService(TodoRepository todoRepository) {
		this.todoRepository = todoRepository;
	}

	public List<TodoEntity> readAllTodo() {
		try {
			return todoRepository.findAllList();
		} catch (Exception e) {
			throw new SqlException("xxxx", HttpStatus.INTERNAL_SERVER_ERROR);
		}
		
	}

	public int createTodo(TodoDTO dto) {
		// 임시 데이터 처리
		dto.setUserId(1);
		return todoRepository.createTodo(dto);
	}
	
	public TodoEntity readTodById(Integer todoId) {
		return todoRepository.findByIdTodo(todoId);
	}

	public int updateTodoById(Integer todoId, TodoDTO todoDTO) {
	
		return todoRepository.updateById(todoId, todoDTO);
	}
	
	public int deleteTodoById(Integer id) {
		return todoRepository.deleteById(id);		
	}
}
```


- service / UserService

```java
package com.tencoding.todo.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.tencoding.todo.dto.UserDTO;
import com.tencoding.todo.repository.entity.UserEntity;
import com.tencoding.todo.repository.mapper.UserRepository;

@Service // IoC 관리 대상 --> 싱글톤 객체 
public class UserService {

	// final 로 선언하는 이유 
	// 불변성 보장 : 한번 초기화 되면 상태 변경 불가능 
	// 스레드 안정상 : final 필드는 스레드 간 공유 될 때 안전
	// 의도 표현 - 불변성 이다. 
	private final UserRepository userRepository; 
	
	@Autowired // Autowired 명시적으로 선언해주게 좋다  
	public UserService(UserRepository userRepository) {
		this.userRepository = userRepository;
	}
	
	/**
	 * 회원 가입 처리 
	 * @param userDTO
	 * @return
	 */
	public int signUp(UserDTO userDTO) {
		// 추가적인 작업 ... 생략 
		return userRepository.signUp(userDTO);
	}
	
	/**
	 * 로그인 처리  
	 * @param userDTO
	 * @return UserEntity
	 */
	public UserEntity signin(UserDTO userDTO) {
		return userRepository.signIn(userDTO);
	}
}
```

- utils / JwtUtil

```java
package com.tencoding.todo.utils;

import java.util.Date;

import javax.crypto.SecretKey;

import org.springframework.stereotype.Component;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jws;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;

@Component // Ioc 대상 된다. 
public class JwtUtil {
	
	// 시크릿 키를 생성 - xxkdk --> 동적 생성 ()
	private final SecretKey key = Keys.secretKeyFor(SignatureAlgorithm.HS512);
	
	private final long EXP_TIME = 86400000L; // 유효 시간 1일  
		
	// 토큰 생성하는 메서드를 만들어야 한다.
	public String generateToken(String userEmail, Integer userId) {
		return Jwts.builder()
				.claim("userEmail", userEmail)
				.claim("userId", userId)
				.setExpiration(new Date(System.currentTimeMillis() + EXP_TIME))
				.signWith(key)
				.compact();		
	}
	
	// 토큰이 유효한지 검증 메서드를 만들어야 한다.
	public Boolean validateToken(String token) {
		try {
			Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(token);
			return true;
		} catch (Exception e) {
			return false;
		}
	}
	
	// 유저에 정보를 뽑고 싶은 메서드를 만들어야 한다. - email 
	public String getUserEmailFromToken(String token) {
		Jws<Claims> claimsJws = Jwts.parserBuilder()
				.setSigningKey(key)
				.build()
				.parseClaimsJws("userEmail");
		
		return claimsJws.getBody().get("userEmail", String.class);
	}
	
	
	// 유저 정보에서 userId를 뽑는 메서드를 만들어 볼 예정 
	public String getUserIdFromToken(String token) {
		Jws<Claims> claimsJws = Jwts.parserBuilder()
				.setSigningKey(key)
				.build()
				.parseClaimsJws("userId");
		
		return claimsJws.getBody().get("userId", String.class);
	}
}
```

### src/main/resources

- mapper / todo.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 

<mapper namespace="com.tencoding.todo.repository.mapper.TodoRepository">

    <!-- 모든 Todo 엔터티를 검색하는 쿼리 -->
	    <select id="findAllList" resultType="com.tencoding.todo.repository.entity.TodoEntity">
	        SELECT * FROM todo_tb
	    </select>

    <!-- Todo 엔터티를 생성하는 쿼리(Mybatise 조건문 사용해보기) -->
    <insert id="createTodo" parameterType="com.tencoding.todo.dto.TodoDTO">
           INSERT INTO todo_tb (title, completed, userId)
    		VALUES (#{title}, 
            <choose>
                <when test="completed == true">1</when>
                <otherwise>0</otherwise>
            </choose>, 
            #{userId});
    </insert>

    <!-- Todo 엔터티를 ID로 검색하는 쿼리 -->
    <select id="findByIdTodo" resultType="com.tencoding.todo.repository.entity.TodoEntity">
        SELECT * FROM todo_tb WHERE id = #{todoId};
    </select>

    <!-- Todo 엔터티를 업데이트하는 쿼리 -->
	<update id="updateById" parameterType="com.tencoding.todo.dto.TodoDTO">
	  UPDATE todo_tb
	  SET title = #{todoDTO.title}, completed = #{todoDTO.completed}
	  WHERE id = #{todoId}
	</update>

    <!-- ID로 Todo 엔터티를 삭제하는 쿼리 -->
    <delete id="deleteById" parameterType="int">
        DELETE FROM todo_tb WHERE id = #{id};
    </delete>

</mapper>
```

- mapper / user.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.tencoding.todo.repository.mapper.UserRepository">
    <insert id="signUp">
        INSERT INTO user_tb (username, email, password) VALUES (#{username}, #{email}, #{password})
    </insert>

    <select id="signIn" resultType="com.tencoding.todo.repository.entity.UserEntity">
        SELECT userId, username, email
        FROM user_tb
        WHERE email = #{email} AND password = #{password}
    </select>
</mapper>
```



