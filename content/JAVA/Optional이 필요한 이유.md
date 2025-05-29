```java
//repository
public interface TestStockRepository extends JpaRepository<TestStock, Long> { 
    Optional<TestStock> findFirstByNameAndIsSoldOutOrderByDateIn(String name, boolean isSoldOut);  
}
```

왜 이렇게 썼는지 같이 공부를 해보자.


먼저 생각해보자,

```java
TestStock findFirstByNameAndIsSoldOutOrderByDateIn(String name, boolean isSoldOut);  
```

자, 이렇게 해도 크게 상관은없어.

즉, 오류가 나지 않는다는거야 - 하지만 이렇게 쓰면 있어도 되고 없어도 된다는것을 서비스에서 처리한다는 거야. 

아주 불편한 상황인거지.

```java
//service
Optional<TestStock> testStock = testStockRepository.findFirstByNameAndIsSoldOutOrderByDateIn(name, false).orElseThrow(CMissingDataException::new)
```

이렇게 말이야. 서비스에서 이런식으로 처리해야한다는거거든?


service: 레포지토리야 갖다줘라~~

repository: 알써 갔다올게! 자, 여기있어.

service: 오케이. 이거 받아서 잘써야겠다.


그저 서비스는 갖다쓰는 아이이기때문에 있고없고의 판단은 repository가 해야한다는것이다



여기까지 선생님과 함께한 수업내용이었다.
근데 잘 이해가 안가서 좀 더 알아봤다.

---


## Optional을 왜 Repository에서 쓰는가?

## 반환 타입에 따른 차이

### (1) 반환 타입이 `TestStock`인 경우

```java
TestStock findFirstByNameAndIsSoldOutOrderByDateIn(String name, boolean isSoldOut);
```

- 이 메서드는 **항상 TestStock 객체를 반환**하겠노라고 약속을 한다.
- 만약 데이터베이스에 해당 조건에 맞는 값이 없으면, 내부적으로 null을 반환할 수밖에 없다.
- 즉, 서비스 계층에서 null 체크를 반드시 해줘야만 하겠지?
- 실수로 null 체크를 빼먹으면, 나중에 NullPointerException이 발생할 수 있다.
- 약속해놓고 이놈이 안준다는 말이다. ...서비스둥절

### (2) 반환 타입이 `Optional<TestStock>`인 경우

```java
Optional<TestStock> findFirstByNameAndIsSoldOutOrderByDateIn(String name, boolean isSoldOut);
```


- 이 메서드는 **있을 수도 있고, 없을 수도 있는 값**을 명확하게 표현해준다.
- 값이 없을 때는 Optional.empty()로 반환되니까, 서비스 계층에서 `.orElseThrow()` 등으로 안전하게 처리할 수 있다는 소리다.
- null을 직접 다루지 않으니, 실수로 인한 NullPointerException 위험이 줄어든다.
- 그러니까 반드시 주겠노라고 지키지도 못할 약속을 하지않는다는 소리다. 얼마나좋은가!

---

## 책임의 분리

- **Repository**는 "해당 조건에 맞는 데이터를 찾아서 줄게~" 역할만 해야한다. 창고지기잖아. 그 일만 하셔야지 쓸데없는 약속하시면되는가.
- **Optional**을 반환함으로써, "이 조건에 맞는 데이터가 없을 수도 있다"는 사실을 서비스 계층에 명확하게 알려준다.
- **Service**는 "데이터가 없으면 어떻게 할지"를 결정한다. 예외를 던질 수도 있고, 기본 값을 사용할 수도 있어요. (직원이 일을 못하겠다고 할때, 어떻게 할지는 서비스가 알아서 할 것이다. 똑똑한 상사님이다.)

---

## 정리하자면!

- **Repository**: "창고에서 오예스 있으면 줄게, 없으면 빈 손이야."
- **Service**: "오예스 받아서 먹을 건데, 없으면 에러를 내야지!"

만약 Repository가 무조건 오예스를 준다고 약속해버리면(즉, Optional이 아니라 TestStock을 반환하면), 실제로 없을 때 문제가 생긴다 - (NullPointerException).

Optional로 반환하면, "없을 수도 있다"는 사실을 코드상에서 명확하게 표현할 수 있다.

- **Optional을 쓰면, 데이터가 없을 수도 있다는 사실을 명확하게 표현할 수 있다.**
- **서비스 계층에서 값이 없을 때의 처리를 안전하게 할 수 있다.**
- **NullPointerException 같은 버그를 예방할 수 있다.**