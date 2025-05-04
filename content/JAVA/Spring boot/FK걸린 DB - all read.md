바로 서비스에 넣는게 아니라 일단 뿌려줄 값 모양부터 잡고 시작하자.

## model만들기 - BuyListItem

```java
@Setter  
@Getter  
public class BuyListItem {  
    private Long id;  
    private LocalDate date;  
    private String name;  
}
```

가공할 그릇이 나왔으니까, 이제는 서비스에서 어떻게 보내줄 것인가를 지정하면된다.


## service에서 메서드만들기
이 메서드는 repository에서 값을 모두 리스트형태로 들고올 것이고 
들고온 다음에 linkedlist에 세팅해서 뿌려줄것이다

```java
public List<BuyListItem> getBuyLists(){  
    List<BuyList> buyLists = buyListRepository.findAll();  
    List<BuyListItem> buyListItems = new LinkedList<>();  
    for( BuyList item : buyLists){  
        BuyListItem buyListItem = new BuyListItem();  
        buyListItem.setId(item.getId());  
        buyListItem.setDate(item.getDate());  
        buyListItem.setName(item.getFamily().getName());  
        buyListItems.add(buyListItem);  
    }  
    return buyListItems;  
}
```

나는 향상된 for문을 이용했다. 
여기서 눈여겨 볼 부분은  `buyListItem.setName(item.getFamily.getName())`인데
왜 jpa가 entity 설계할 때, Long 이아니라 Family자체를 요구했는지 잘 알게되었다



## 마지막으로 controller만 세팅해주면 끝.

```java
@GetMapping("/buylist/all")  
public List<BuyListItem> getBuyLists() {  
    return buyListService.getBuyLists();  
}
```

