[[Paging]] 에 대한 실전 예제이다



## 1. controller에서 param으로 값 받아오기

사실 findAll()이니까 원래는 param이 없었으나, paging기능을 넣기위해 param값을 가져오기로한다. 안가져오면 그냥 1이 세팅될 수 있도록 만든다

```java
//controller

@GetMapping("/all")  
public List<BookItem> getBooks(  
        @RequestParam(required = false, defaultValue = "1") int pageNum  
){  
    return bookService.getBooks(pageNum);  
}
```

- default해놓은 곳이 있는 여기는 url에서 긁어오는거라서 무조건 문자로 써야한다


## 2. 서비스

```java
public List<BookItem> getBooks(int pageNum) {  
  
    Pageable pageable = PageRequest.of(pageNum - 1, 10);  
    Page<Book> books = bookRepository.findAll(pageable);  
    List<BookItem> bookItems = new LinkedList<>();  
    for( Book book : books.getContent() ) {  
        BookItem bookItem = new BookItem();  
        bookItem.setId(book.getId());  
        bookItem.setTitle(book.getTitle());  
        bookItem.setAuthor(book.getAuthor());  
        bookItem.setLoaned(book.getLoaned());  
        bookItems.add(bookItem);  
    }  
    return bookItems;  
}
```

setter를 빼지않아서 예쁘진않지만 그냥 이해해줘.

이제는 List로 받아오는 것이 아니라 `Page<Book>`으로 받아온단 얘긴데,

repository에 뭐 다른걸 할필요는 없어. 왜냐하면 그 안에 pageable를 그냥 넘기면되거든

```java
Pageable pageable = PageRequest.of(pageNum - 1, 10);  
```

여기서 말이야. 지금 new를 안치잖아? 그런 이유가 있어

new를 치면 그거에 대한 모든것을 사용하겠다는 의미이고,

이처럼 그냥 사용하면 of만 쓰겠다는것이다. 


---

엄청 간단하네. 딱 여기까지 작업하고나면 기본적으로 paging기능을 사용할 수 있게 되었다네!

