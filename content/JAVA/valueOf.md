**valueOf**는 Java에서 다양한 데이터 타입을 문자열(String)로 변환하거나, 
문자열을 해당 타입의 객체로 변환할 때 사용하는 메서드이다

## **1. String.valueOf()**

- **기능:**  
    다양한 기본형 데이터(int, long, double, boolean 등)나 객체(Object)를 문자열로 변환한다.
- **오버로딩:**  
    String 클래스에는 여러 타입에 대해 valueOf 메서드가 오버로딩되어 있다. (예: int, char, long, float, double, boolean, Object, char[] 등)

내가 코딩테스트를 하면서 만난 `String.valueOf()`는 이에 해당한다

## **2. Wrapper 클래스의 valueOf()**

- **기능:**  
    문자열을 해당 타입의 객체로 변환한다.
- **특징:**
    - 반환값이 객체(예: Integer, Double 등)다.
    - `parseInt()`와 달리 객체를 반환하며, null 입력에 대해 예외가 발생할 수 있다.

## **toString()과의 차이**

- `toString()`은 객체의 문자열 표현을 반환하지만, 객체가 null이면 NullPointerException이 발생한다.
- `String.valueOf()`는 null 객체를 입력받아도 "null"이라는 문자열을 반환한다. 😧


## **요약**

- **String.valueOf()** : 다양한 타입을 안전하게 문자열로 변환할 때 사용한다.
- **Wrapper.valueOf()** : 문자열을 해당 타입의 객체로 변환할 때 사용한다.
- **null 처리**가 필요하다면 `String.valueOf()`를 사용하는 것이 안전하다.