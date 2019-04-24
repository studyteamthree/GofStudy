# 생성패턴(Creational Pattern) - Builder Pattern



#### 목적

> 객체의 construction 과정과 객체의 representation을 분리
>
> 이렇게 분리함으로 하나의 construction 과정을 이용해 서로 다른 형태의 representation 을 이용할 수 있음 => 하나의 Director로 상위 Builder에 표현된 다른 하위 Builder를 이용할 수 있음.
>
> 복잡한 객체의 단계별 생성
>
> 각기 다른 능력을 가진 객체를 생성하는데 있어서 통일 된 방식을 제공한다는 것.





### UML Diagram

![1556019481358](https://user-images.githubusercontent.com/18109075/56580905-be500000-660e-11e9-9cb0-a543b3b4dad7.png)

* **Builder**
   * Product 객체의 부분을 생성하기위한 추상 인터페이스를 지정합니다. 
* **ConcreteBuilder**
   * Builder 인터페이스를 구현하여 제품의 일부를 구성하고 어셈블합니다. 
  * 작성한 표현을 정의하고 추적합니다
  * 제품 검색을위한 인터페이스를 제공합니다. 
* **Director**
  * Builder 인터페이스를 사용하여 객체를 생성합니다. 
* **Product**
  - 생성중인 복합 객체를 나타냅니다. ConcreteBuilder는 제품의 내부 표현을 작성하고 조립 과정을 정의합니다. 
  - 최종 결과로 부품을 조립하기위한 인터페이스를 포함하여 구성 부품을 정의하는 클래스를 포함합니다.



### Builder Pattern 을 RTF 문서판독기에 적용한 모습

![1556019620816](https://user-images.githubusercontent.com/18109075/56580911-c14af080-660e-11e9-82f4-8abf05bea156.png)



**적용 내용**

- RTFReader = Director // Product = 결과물 // Builder = TextConverter // ConcreteBuilder = 각 Converter

- 하나의 생성과정에 다른 능력을 가진 객체를 생성하기 위해 인터페이스로 구현된 TextConverter가 필요

- 그 인터페이스를 통해 상세 구현체를 알 필요 없이 통일된 방식 그대로를 생성해 줄 뿐(상세 구현체는 client 결정)

- ##### 다른 예로 하나씩 따로 따로 내부의 객체를 생성해서 하나로 묶거나 리턴하는 식으로도 많이 활용된다.



### 자바에서의 빌더 패턴

> **회원가입** 에 필요한 **Member** **객체**를 만드는데 아이디와 패스워드만 필수정보이고, 나머지는 입력하지 않아도 상관없을때 빌더패턴을 유용하게 활용할 수 있다.



``` java
public interface Buildable <T>{
	T build();
}
```

``` java

public class Member {
	// 회원가입 입력양식
	private String id;
	private String pw;
	private String name;
	private String email;
	private int age;
	private String phone;
	private String address;
	
	public Member() {}
	
	public static class Builder implements Buildable<Member>{
		// 필수적으로 필요한 정보
		private final String id;
		private final String pw;
		
		// 선택적으로 필요한 정보
		private String name;
		private String email;
		private int age;
		private String phone;
		private String address;
		
		// 필수적으로 필요한 정보들에 대한 생성자
		public Builder(String id, String pw) {
			this.id = id;
			this.pw = pw;
		}
		
		// 선택적으로 필요한 정보들에 대한 정보를 받고 미완성된 객체들을 전달할 메소드
		public Builder name(String name) {
			this.name = name;
			return this;
		}
		
		public Builder email(String email) {
			this.name = email;
			return this;
		}
		
		public Builder age(String age) {
			this.name = age;
			return this;
		}
		
		public Builder phone(String phone) {
			this.name = phone;
			return this;
		}
		
		public Builder address(String address) {
			this.name = address;
			return this;
		}
		
		// 메소드의 최종에 사용해 완성된 객체를 어떤 상황에서도 반환
		@Override
		public Member build() {
			// TODO Auto-generated method stub
			return new Member(this);
		}
		
	}
	
	public Member(Builder builder) {
		this.id = builder.id;
		this.name = builder.name;
		this.email = builder.email;
		this.age = builder.age;
		this.phone = builder.phone;
		this.address = builder.address;
	}

	
}
```



### 활용 결과

``` java
Member member1 = new Member.Builder("문상수", "1234")
                            .age("27")
                            .address("강남")
                            .email("mshero7")
                            .build();
```





## 빌더 패턴의 장단점

- 장점 
  - 복합 객체가 생성되는 과정을 캡슐화합니다.
  - 여러 단계와 다양한 절차를 통해 다양한 객체를 만들 수 있다.
  - 제품의 내부 구조를 보호할 수 있다.
  - 추상 인터페이스만 볼 수 있기 때문에 구현한 코드를 쉽게 바꿀 수 있다.



- 단점
  - 사용자는 제품의 내부 구조를 정의한 크래스는 전혀 모른채 빌더와의 상호작용만으로 구성됨
  - 복합 객체 구조를 구축하기 위한 용도로 많이 쓰입니다.
  - 팩토리를 사용하는 경우에 비해 객체를 만들기 위해서 클라이언트에 대해 더 많이 알아야 함.



참고자료

- <https://hamait.tistory.com/847>
- <https://keichee.tistory.com/176>

- <https://johngrib.github.io/wiki/builder-pattern/#%EC%9E%A5%EC%A0%90-2>

- 자바 생성자로 인한 문제를 해결하기 위한 참고할만한 자료
  - <https://asfirstalways.tistory.com/350>
