# Composite Pattern

## 사용 목적
##### 여러 개의 객체들로 구성된 복합 객체(Group instance)와 단일 객체(Single instance)를 클라이언트에서 구별 없이 다루기 위해.

- 전체 - 부분의 관계(Ex. Directory-File)를 갖는 객체들 사이의 관계를 정의할때 유용하다.

- 또한, 클라이언트는 전체와 부분을 구분하지 않고 동일한 인터페이스를 사용할 수 있다.

***
## UML Diagram
![alt_text](https://gmlwjd9405.github.io/images/design-pattern-composite/composite-pattern.png)

* Component
 - 구체적인 부분
 - 즉, Leaf 클래스와 전체에 해당하는 Composite 클래스에 공통 인터페이스를 정의.
* Leaf
 - 구체적인 부분 클래스
 - Composite 객체의 부품으로 설정한다.
* Composite
 - 전체 클래스
 - 복수 개의 Component를 갖도록 정의
 - 그러므로, 복수 개의 Leaf, 심지어 복수 개의 Composite 객체를 부분으로 가질 수 있다.


## 예제
 ![alt_text](https://gmlwjd9405.github.io/images/design-pattern-composite/composite-solution1.png)

``` java
// Component 클래스 - 서브 클래스들에게 동일한 목적의 기능을 강제화 한다.
public abstract class ComputerDevice {
  public abstract int getPrice();
  public abstract int getPower();
}
```


``` java
// Leaf 클래스 - 부모(Component) 클래스에게 동일한 기능을 물려받은 단일 객체
public class Keyboard {
    //
	private int price;
	private int power;
	//
	private String kind;
	private int keyCount;
  public Keyboard(int power, int price) {
    this.power = power;
    this.price = price;
  }
  public int getPrice() { return price; }
  public int getPower() { return power; }
}
public class Body { 동일한 구조 }
public class Monitor { 동일한 구조 }
public class Speaker { 동일한 구조 }
```

``` java
// 복수 개의 Component를 가질 수 있고, 자신 또한 Component 자식인 Composite 클래스
public class Computer implements ComputerDevice {
	// 복수 개의 Leaf 객체를 가리킴
	private List<ComputerDevice> components = new ArrayList<ComputerDevice>();

	// Leaf 객체를 Composite 클래스에 추가
	public void addComponent(ComputerDevice component){
		components.add(component);
	}
	// Leaf 객체를 Composite 클래스에서 제거
	public void removeComponent(ComputerDevice component){
		components.remove(component);
	}

	// 전체 가격을 포함하는 각 부품의 가격을 합산
	@Override
	public int getPrice() {
		int price = 0;
		for(ComputerDevice component : components){
			price += component.getPrice();
		}
		return price;
	}
	// 전체 소비 전력량을 포함하는 각 부품의 소비 전력량을 합산
	@Override
	public int getPower() {
		int power = 0;
		for(ComputerDevice component : components){
			power += component.getPower();
		}
		return power;
	}
}
```
```java
public class Client {
	public static void main(String[] args) {
		// 컴퓨터의 부품으로 Keyboard, Body, Monitor 객체를 생성
		Keyboard keyboard = new Keyboard(5, 2);
		Body body = new Body(100, 70);
		Monitor monitor = new Monitor(20, 30);
		Speaker Speaker = new Speaker(15, 25);

		// Computer 객체를 생성하고 addComponent()를 통해 부품 객체들을 설정
		Computer computer = new Computer();
		computer.addComponent(keyboard);
		computer.addComponent(body);
		computer.addComponent(monitor);
		computer.addComponent(Speaker);

		// 컴퓨터의 가격과 전력 소비량을 구함
		System.out.println("컴퓨터의 가격은 : " + computer.getPrice() + "만원");
		System.out.println("컴퓨터의 소비 전력은 : " + computer.getPower() + "W");
	}
}
```


## 참고 문헌
- https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html
- https://effectiveprogramming.tistory.com/entry/Composite-%ED%8C%A8%ED%84%B4?category=660013
- https://keichee.tistory.com/184?category=834910
- Head First Design Pattern
