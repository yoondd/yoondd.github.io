
Json을 자바 객체로 변환하거나 자바 객체를 json으로 변환해준다


# 객체를 json으로

```java
ObjectMapper objectMapper = new ObjectMapper();

String json = objectMapper.writeValueAsString(burger);
```

여기서  burger는 미리 만들어둔 객체이다
저 객체안의 내용을 json으로 반환해준다

## json을 좀 더 예쁘게 보고싶다면

```java
JsonNode jsonNode = objectMapper.readTree(json);
System.out.println(jsonNode.toPrettyString);
```

---

# json을 객체로

```java
ObjectMapper objectMapper = new ObjectMapper(); // 준비

Burger burger = objectMapper.readValue(json, Burger.class);
```

이와같이 json을 가지고 어떤 클래스타입으로 변환할건지 맞춰주면 정확히 그 객체로 바꾸어준다


---

# json을 직접 만들기

```java
ObjectNode objectNode = objectMapper.createObjectNode();
objectNode.put("name", "맥도날드 슈비버거");
objectNode.put("price", 5500);
```

위와같이 하면 내가 원하는 것만 만들수가 있고. 배열도 넣을 수가 있다

```java
ArrayNode arrayNode = objectMapper.createArrayNode();
arrayNode.add("통새우 패티");
arrayNode.add("순쇠고기 패티");
arrayNode.add("토마토");

objectNode.set("ingredients", arrayNode);

String json = objectNode.toString();    
```
