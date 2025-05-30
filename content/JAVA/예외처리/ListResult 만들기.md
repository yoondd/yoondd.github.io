
## 1. `ResponseSerivce`

```java
public static <T> ListResult<T> getListResult(List<T> list) {  
    ListResult<T> result = new ListResult<>();  
    result.setList(list);  
    setResult(result, true, ResultCode.SUCCESS);  
    return result;  
}
```


## 2. `TestStockService`

```java
public List<TestStock> getStocks(){  
    return testStockRepository.findAll();  
}
```

원래 정석대로라면 item을 만들어야하는데 그냥 편의상 이렇게 주는 것 뿐이야


## 3. `TestController`

```java
@GetMapping("/all")  
public ListResult<TestStock> getAll(){  
    return ResponseService.getListResult(testStockService.getStocks());  
}
```


결국 제네릭 사용법을 익혀서 여기까지 완성했다고 보면 된다.

길고도 긴 여정이었다.


1. 성공 리턴
2. 실패 리턴
3. 성공 후 data까지 넣어서 단수 리턴
4. 성공 후 list까지 넣어서 복수 리턴

여기까지 해냈다고 보면된다.