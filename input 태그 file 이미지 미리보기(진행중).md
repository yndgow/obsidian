
```js
document.getElementById('product-thumbnail').addEventListener('change', function () {
  previewImg(this);
  //let tempImgs = document.getElementsByClassName('tmpImg');
});


const previewImg = (input) => {
  if (input.files && input.files[0]) {
	let fr = new FileReader();
	let thumbImg = document.createElement('img');
	thumbImg.style.width = '100px';
	thumbImg.className = 'tempImg';
	fr.onload = (e) => {
	  thumbImg.src = e.target.result;
	};
	fr.readAsDataURL(input.files[0]);
	document.getElementById('thumbnailTd').append(thumbImg);
  } else {
  }
};
```
