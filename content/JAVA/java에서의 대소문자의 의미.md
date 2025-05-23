
### 왜 어노테이션은 첫 글자가 대문자일까?

결국 **클래스**니까.
어노테이션은 런타임(실행)때도 관여를한다.

커스텀 어노테이션을 단다음에, 런타임시에 자동작동하도록 만든다.
그래서 로그가 찍히게 만들 수 있다.
물론 컴파일시에도 동작하게 할 수 있는데, 그건 내가 알아서 지정해서 작동하게 만들 수 있다.
어노테이션이라고해도 정해져있는게 아니라 결국 내가 인터페이스(클래스)를 만들어서 쓰면된다는 말이다.


---

어노테이션의 첫 글자가 대문자인 이유는 결국 어노테이션이 **클래스 또는 인터페이스 형태로 정의되기 때문**이다.. 자바(Java)를 비롯한 많은 언어에서 클래스 이름은 관례적으로 첫 글자를 대문자로 작성하는 명명 규칙을 따른다. 어노테이션도 `@interface`라는 키워드를 사용해 정의하는 일종의 인터페이스이므로, 클래스와 같은 명명 규칙을 적용받아 첫 글자가 대문자이다.


또한, 어노테이션은 단순히 코드에 표시하는 메타데이터를 넘어서 런타임에도 동작할 수 있다. 커스텀 어노테이션을 만들고 런타임 시 자동으로 작동하도록 설정하면, 예를 들어 로그를 찍거나 특정 동작을 수행하게 할 수 있다. 컴파일 시에도 어노테이션 처리기를 통해 동작하도록 지정할 수 있으므로, 어노테이션은 *단순한 주석 이상의 역할을 하며 결국 클래스처럼 동작하는 개념*과 일맥상통한다.

[[lombok은 언제 작동하는가?]]

요약하면,

- 어노테이션은 `@interface`로 정의되는 일종의 인터페이스(클래스)이다.
- 클래스 이름은 자바 명명 규칙상 첫 글자가 대문자이다.
- 따라서 어노테이션 이름도 첫 글자가 대문자로 시작한다.
- 어노테이션은 런타임이나 컴파일 타임에 작동할 수 있어, 단순 표시 이상의 기능을 가진다.

이러한 이유로 어노테이션 이름은 첫 글자가 대문자로 시작하는 것이 표준이며, 이는 클래스 명명 규칙과 일관성을 유지하기 위함이다.

---

프로그래밍 언어, 특히 컴파일 언어에서 첫 글자의 대소문자는 **식별자의 의미와 역할을 구분하는 데 중요한 역할**을 한다.

## 대소문자가 갖는 의미

- **대부분의 현대 프로그래밍 언어는 대소문자를 구분(case-sensitive)합니다.** 예를 들어, `Variable`과 `variable`은 서로 다른 식별자로 인식된다.
- 대문자로 시작하는 이름은 보통 **클래스, 인터페이스, 어노테이션 같은 타입(type) 식별자**를 나타내는 데 사용된다. 예를 들어 자바, C#, 파이썬 등에서 클래스 이름은 PascalCase(각 단어 첫 글자 대문자) 규칙을 따른다.
- 반면, 소문자로 시작하는 이름은 주로 **변수, 함수, 메서드, 속성 등 인스턴스나 메서드 단위 식별자**에 사용된다. 예를 들어 자바의 변수명, 메서드명은 camelCase(첫 글자 소문자, 이후 단어 첫 글자 대문자)로 작성하는 것이 관례이다.
- 이러한 명명 규칙은 **가독성 향상과 코드 구조의 명확한 구분**을 위해 널리 쓰인다. 대문자로 시작하는 이름을 보면 개발자는 해당 식별자가 타입임을 직관적으로 알 수 있다..
- 일부 언어 또는 환경에서는 대소문자 구분 여부가 다르기도 하지만, 대다수 언어는 대소문자를 구분하여 더 많은 고유 식별자를 만들고, 명확한 코드 작성을 지원한다.

## 요약

- 첫 글자 대문자: 주로 클래스, 인터페이스, 어노테이션 등 타입 식별자에 사용
- 첫 글자 소문자: 변수, 함수, 메서드 등 인스턴스 식별자에 사용
- 대소문자 구분은 언어에 따라 다르지만, 대부분의 현대 언어는 구분하여 코드의 명확성과 가독성을 높임
- 명명 규칙은 개발자 간 일관된 코드 스타일과 유지보수성을 위해 중요함

따라서 프로그래밍 언어에서 첫 글자의 대소문자는 단순한 표기 차원이 아니라, **코드의 의미와 구조를 구분하는 중요한 관례이자 규칙**이다. 이는 어노테이션이 클래스처럼 대문자로 시작하는 이유와도 일맥상통한다.
