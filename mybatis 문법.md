# mybiats 문법

## 1. foreach

1. INSERT
```xml
<insert id= "insert">
	INSERT INTO product_image_tb(product_id, image, is_thumbnail, created_at) values
	<foreach collection="list" item="item" separator=",">
		(
			#{item.productId}, #{item.image}, #{item.isThumbnail}, now()
		)
	</foreach>
</insert>
```

INSERT ... VALUES (), (), () 를 만들어준다. VALUES 뒤부터 반복 시켜야하고 구분자(,)를  넣는다.

2. UPDATE
```xml
<update id="updateById">
	<foreach collection="list" item="imgList">
		UPDATE product_image_tb
		SET image = #{imgList.image},
		created_at = now()
		WHERE id = #{imgList.id};
	</foreach>
</update>
```

List 를 업데이트 하는경우 item에는 사용할 변수명을 쓰고, collection 에는 list 형태임을 밝혀준다.

 ⚠️ UPDATE 를 mybatis에서 foreach를 사용해서 반복시키고자한다면 (아마도 위의 INSERT도 적용되겠지만) 
```
 jdbc:mysql://localhost:3306/test?allowMultiQueries=true
```
 

이와같이 allowMultiQueries=true 옵션을 넣어야한다. 이는 아마도 Mysql에서만 적용가능한 옵션인거 같다. 데이터베이스 차이가 있다면 차라리 sql을 service에서 직접 반복시키는게 낫지 않을까?