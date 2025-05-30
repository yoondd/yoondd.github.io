빌더패턴의 복수 R, 단수 R에 대해 알아볼까?

item이나 response부분에다가 적용을 한번 해볼까?


## 1. 복수R

```java
// model/ProductItem

@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class ProductItem{
	private String id;
	private String productType; //enum이지만 string으로 뿌릴거다
	private String name;
}
```

여기서는 `@NotNull`같은건 필요하지않다.

내가 넣는거니까 검증할 필요가없거든.


주겠다고 이렇게 만들어 둔 것은 ==필수==라는 소리다. 넣어주기로한건 넣어줘야지


```java
// model/ProductItem

private ProductItem(Builder builder){

}

public static class Builder implements Co~<ProductItem> {
	@Override
	public ProductItem build(){
		return new ProductItem(this); 
	}

}
```

this를 쓸려면 위에 생성자가필요했다

자, 이렇게 틀을 만들어 놓고 작업을 시작해라


```java
public static class Builder implements Co~<ProductItem> {

	private final String id;
	private final String productType;
	private final String name;

	public Builder(Product product){
		this.id = product.getId();
		this.poductType = product.getProductType().getName();
		this.name = product.getName();
	}

	@Override
	public ProductItem build(){
		return new ProductItem(this); 
	}

}
```


자, 여기까지 했으면 이제  위에 비워둔 생성자 채우자구~


```java
private ProductItem(Builder builder){
	
}

```

08:59