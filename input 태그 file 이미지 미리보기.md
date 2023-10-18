
```js
// 이미지 미리보기
function readURL(input) { // input type="file" 가져오기
	if (input.files) { // 파일이 선택된다면
	const reader = new FileReader(); // 파일리더 생성
	reader.onload = function(e) {
	input.previousElementSibling.src = e.target.result;
	}
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
