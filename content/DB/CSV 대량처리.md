

## 연산자 선택

버퍼에서 라인으로 하나씩 보내준다(똥덩어리)

이 행동을 하나씩 반복할 것이다

`+=` 연산자는 `line = line + 무언가` 를 의미한다

그래서 `+=`를 이용하면 용량이 너무 커져버린다. 

버퍼의 똥덩어리들을 효율적으로 가져오려면 `String line = 똥덩어리` 하나만 넣고

처리가 끝나면 두번째 똥덩어리를 받아와야한다

기존에 있던 똥덩어리 버리고 두번째것만 넣는 것이 좋다

계속 더하는게 아니라 한줄씩 받아서 처리해야한다는 소리다 

즉, `+=`이 아니라, `=`을 이용해야한다는 소리다




## 반복문 만들기


`while`문을 사용할 건데, 더이상 던져줄 똥덩어리가 없을때까지 반복할 것이다라고 말을 해야겠다

`while( (line=bufferedReader.readLine()) != null )`

버퍼에서 꺼내는 똥덩어리가 null이 아닐때까지 반복하겠다는 소리다.


줄 번호도 `+1`해줘야한다. 

```java
public void setRentCarByFile(MultipartFile multipartFile) throws IOException{

	String line = 0;
	int index = 0;

	While((line=bufferedReader.readLine()) != null ){
		if ( index >0 ){
			System.out.println(line);
			System.out.println(index);
			index++;
		}
		
	}

}
```

- 조건문을 건 이유는 첫줄을 thead같은거라서 그렇다 (맨 윗 줄 날리려고)

이렇게까지 진행하면 데이터는 중간중간 `,`가 삽입된 상태로 들어온다

`13호1234,모닝,20000` 이런식으로말이다.

이걸 잘라서 처리해줘야한다는 소리다




## `split`을 이용해서 자른다

사람이 보기편하게 잘라주는 아이라고 생각하면된다.

위 속 음식물을 쪼개는 스프라이트처럼 우리도 문자열을 쪼개고 박살내자

```java
public void setRentCarByFile(MultipartFile multipartFile) throws IOException{

	String line = 0;
	int index = 0;

	While((line=bufferedReader.readLine()) != null ){
		if ( index >0 ){
			String[] cols = line.split(","); //스프라이트 샤워
			System.out.pringln(cols[0]);
			System.out.pringln(cols[1]);
			System.out.pringln(cols[2]);
		}
		
	}

}
```

잘 짤렸는지 확인해보면 실제 화면에 예쁘게 나오는 것을 볼 수 있다

```text
13호4445
모닝
20000
```





## 빈 필드로 인한 오류 방지하기

모든 줄이 3칸이라는 확신이 정말 있나요

아니 없지... 그러니까 위 코드처럼 0,1,2로 쓰는건 너무 비효율적이야

null이 들어오면 그건 캐스팅할수가없다 - 아무것도 없는 것을 어떻게 캐스팅해... 

if문에서 하나 더 넣어야겠다

null이 들어왔을때 어떻게 처리해야하는지 말이다

이 배열의 길이를 받아서 처리하는게 맞겠다


```java
public void setRentCarByFile(MultipartFile multipartFile) throws IOException{

	String line = 0;
	int index = 0;

	While((line=bufferedReader.readLine()) != null ){
		if ( index >0 ){
			String[] cols = line.split(","); //스프라이트 샤워
			if( cols.length == 3){
				System.out.pringln(cols[0]);
				System.out.pringln(cols[1]);
				System.out.pringln(cols[2]);
			}
			
		}
		
	}

}
```

이렇게 처리하면 그 오류까지도 대응할 수 있겠군.



## 대량 처리

렌터카 데이터는 4개가 아니잖아. 데이터가 엄~청 많다는 소린데.

엑셀파일을 한번에 올려서 처리해볼까?

올린다? 이 표현 자체가 바로 CRUD의 Create를 말하는거네.

수십만개의 데이터를 한번에 처리하겠다는 소린데.

Entity를 하나 새로 만들어야겠다


컬럼별로 column를 잘 만들어두고

```java
@Entity
@Getter
@Setter
public class RentCar{
	

}
```