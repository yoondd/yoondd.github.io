
## 자바 Builder 패턴으로 모델 만드는 순서

## 1. 필드 선언한다

```java
private String id;
private String productType;
private String name;
private String productImageUrl;

```

모델에서 사용할 필드들 선언한다.

---

## 2. Builder 내부 클래스 만든다

정적(static) 내부 클래스로 Builder 만든다. 생성자에서 값 받아온다.

```java
public static class Builder implements CommonModelBuilder<ProductResponse> {
    private final String id;
    private final String productType;
    private final String name;
    private final String productImageUrl;

    public Builder(Product product) {
        this.id = product.getId();
        this.productType = product.getProductType().getName();
        this.name = product.getName();
        this.productImageUrl = product.getProductImageUrl();
    }

    @Override
    public ProductResponse build() {
        return new ProductResponse(this);
    }
}

```

---

## 3. 생성자에서 Builder 값 할당한다

ProductResponse 생성자는 Builder 받아서 필드에 값 할당한다.

```java
private ProductResponse(Builder builder) {
    this.id = builder.id;
    this.productType = builder.productType;
    this.name = builder.name;
    this.productImageUrl = builder.productImageUrl;
}

```

---

## 4. build() 메소드 구현한다

Builder에서 build() 메소드 만들어서 실제 모델 객체 만든다.

```java
@Override
public ProductResponse build() {
    return new ProductResponse(this);
}

```

---

## 전체 코드 예시

```java
@Getter
@NoArgsConstructor
public class ProductResponse {

    private String id;
    private String productType;
    private String name;
    private String productImageUrl;

    private ProductResponse(Builder builder) {
        this.id = builder.id;
        this.productType = builder.productType;
        this.name = builder.name;
        this.productImageUrl = builder.productImageUrl;
    }

    public static class Builder implements CommonModelBuilder<ProductResponse> {
        private final String id;
        private final String productType;
        private final String name;
        private final String productImageUrl;

        public Builder(Product product) {
            this.id = product.getId();
            this.productType = product.getProductType().getName();
            this.name = product.getName();
            this.productImageUrl = product.getProductImageUrl();
        }

        @Override
        public ProductResponse build() {
            return new ProductResponse(this);
        }
    }
}

```

---

## 정리한다

이렇게 하면 Builder 패턴으로 모델을 깔끔하게 만들 수 있다. 생성자 파라미터 많거나 복잡할 때 유용하다.

모델은 비즈니스 데이터를 담는 역할을 한다고 보면 된다. 