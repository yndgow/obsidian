# mybiats 문법

## foreach

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

List 를 업데이트 하는경우 item에는