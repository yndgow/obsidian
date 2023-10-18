
참고문서
https://developer.mozilla.org/ko/docs/Web/API/FileReader


```js
// 이미지 미리보기
function readURL(input) { // input type="file" 가져오기
	if (input.files) { // 파일이 선택된다면
	const reader = new FileReader(); // 파일리더 생성
	// 이벤트의 핸들러. 읽기 동작이 성공적으로 완료 되었을 때마다 발생합니다.
	reader.onload = function(e) { 
		input.previousElementSibling.src = e.target.result;
	}
		// 지정된 내용을 읽기 시작합니다  완료되면 속성 에 파일 데이터를 나타내는 
		// URL이 `result`포함됩니다
		reader.readAsDataURL(input.files[0]);
	} else {
		input.previousElementSibling.src = "";
	}
}

if(currentTarget.classList.contains('updateFile')){
	currentTarget.addEventListener('change', function(e){
		console.log(this.previousElementSibling);
		readURL(e.target);
	});
}
```
