-  ---
   title: "Bridge Pattern"
   date: 2019-04-26 20:00:00 +0900
   categories: Gof Study
   ---

   ## 구조패턴(Structural Pattern) - Bridge Pattern

   

   #### 목적

   >  여러 클래스 간의 강한 결합을 제거하기 위해 구현부와 추상층을 분리하여 각자 독립적으로 변형할 수 있게 하는 패턴.

   

   ### UML Diagram

   ![1556276032248](assets/img/1556276032248.png)

   

   -  Abstraction : 추상적 개념에 대한 인터페이스를 제공하고 객체 구현자(Implementator)에 대한 참조자를 관리합니다. = (MorseCode)
   -  RefinedAbstraction : 추상적 개념에 정의된 인터페이스를 확장합니다.(=PrintMorseCode)
   -  Implementor : 구현 클래스에 대한 인터페이스를 제공합니다. Implementor는 기본적인 구현 연산을 수행하고, Abstraction은 더 추상화된 서비스 관점의 인터페이스를 제공합니다.(=DefaultMSF)
   -  ConcreteImplementor : Implementor 인터페이스를 구현하는 것으로 실제적인 구현 내용을 담았습니다.(=PrintMorseCode)

   

   #### 실전 문제 

    모스부호통신에 dot, dash, space로 이뤄진 문자들의 조합으로 통신을 한다고 한다. 각 기기마다 표현되는 방식이 다르다고 할때 어떻게 구현하면 될까.

   

   #### CASE 1 : Bridge Pattern을 사용하지 않은 경우

   첫번째 모스부호기는 +,-,(공백) 으로 표현

   ```java
   public class MorseCode1 {
   	public void dot() {
   		System.out.print("+");
   	}
   	public void dash() {
   		System.out.print("-");
   	}
   	public void space() {
   		System.out.print(" ");
   	}
   }
   ```

   

   두번째 모스부호기는 삑,삐,삡 소리로 표현

   ```java
   public class MorseCode2 {
   	public void dot() {
   		System.out.println("삑");
   	}
   	public void dash() {
   		System.out.println("삐");
   	}
   	public void space() {
   		System.out.println("삡");;
   	}
   }
   ```

   

   각 모스부호기를 활용하기 위해서 PrintMorseCode는 MorseCode1 이나 MorseCode2를 상속받아야한다.

   ```java
   public class PrintMorseCode extends MorseCode2 {
   	// 문자를 나타내기 위한 방식.
   
   	public PrintMorseCode a() {
   		dot();dash();space();
   		return this;
   
   	}
   	public PrintMorseCode r() {
   		dot();dash();dot();space();
   		return this;
   
   	}
   	public PrintMorseCode m() {
   		dash();dash();space();
   		return this;
   
   	}
   }
   ```

   

   이런식으로 사용하게 되면 기능이 추가되거나, PrintMorseCode에 지속적인 수정이 필요하게 될 수 있다. 이런 문제점을 해결하기 위해 기능과 구현을 별도의 클래스로 정의해서 분리하는 방법을 통해 확장 설계에 용이하게끔 한다.

   

   ### CASE 2 : Bridge Pattern을 활용한 방법

   MorseCode안에 어떤 문자를 어떻게 해석할지(기능=MorseCodeFunction), MorseCode를 어떻게 사용할지(구현)을 분리해 클래스(PrintMorseCode)를 구현해봤다.

   ```java
   public interface MorseCodeFunction {
   	public void rawDot();
   	public void rawDash();
   	public void rawSpace();
   }
   ```

   ```java
   public class MorseCode {
   	MorseCodeFunction function;
   	
   	public MorseCode(MorseCodeFunction function) {
   		this.function = function;
   	}
   	
   	public void setFunction(MorseCodeFunction function) {
   		this.function = function;
   	}
   	
   	public void dot() {
   		function.rawDot();
   	}
   	public void dash() {
   		function.rawDash();
   	}
   	public void space() { 
   		function.rawSpace();
   	}
   }
   
   ```

   

   ```java
   public class DefaultMSF implements MorseCodeFunction{
   	@Override
   	public void rawDot() {
   		System.out.print("+");
   	}
   
   	@Override
   	public void rawDash() {
   		System.out.print("-");
   	}
   
   	@Override
   	public void rawSpace() {
   		System.out.print(" ");
   	}
   	
   }
   ```

   

   ```java
   public class PrintMorseCode extends MorseCode {
   	
   	public PrintMorseCode(MorseCodeFunction function) {
   		super(function);
   	}
   	
   	// 문자를 나타내기 위한 방식.
   	public PrintMorseCode g() {
   		dash();dash();dot();space();
   		return this;
   	}
   	public PrintMorseCode a() {
   		dot();dash();space();
   		return this;
   
   	}
   	public PrintMorseCode r() {
   		dot();dash();dot();space();
   		return this;
   
   	}
   	public PrintMorseCode m() {
   		dash();dash();space();
   		return this;
   
   	}
   }
   ```

   

   ### 장점

   -  구현을 인터페이스에 완전히 결합시키지 않았기 때문에 구현과 추상화된 부분을 분리시킬 수 있다.
   -  추상화된 부분과 실제 구현 부분을 독립적으로 확장 할 수 있다.
   -  추상화된 부분을 구현한 구상 클래스를 바꿔도 클라이언트 쪽에는 영향을 끼치지 않는다.

   

   ### 사용법 및 단점

   -  여러 플랫폼에서 사용해야 할 그래픽스 및 윈도우 처리 시스템에서 유용하게 쓰인다.
   -  인터페이스와 실제 구현부를 서로 다른 방식으로 변경해야 하는 경우에 유용하게 쓰인다.
   -  디자인이 복잡해진다는 단점이 있다.

   

   ### 참고자료

   -  <https://sexycoder.tistory.com/111>
   -  <https://www.youtube.com/watch?v=YrnXcoSvgyE>