## **Interpreter Pattern**

---



**인터프리터 패턴이란?**

문장을 해석할 때 사용하는 패턴. 해석기, 즉 간이언어를 만들기 위한 패턴. 언어 문법이나 표현을 평가할 수 있는 방법을 제공. (행동패턴)

특정 컨텍스트를 해석하도록 지시하는 표현 인터페이스를 구현하는 것도 포함. 이 패턴은 SQL구문분석, 기호처리엔진 등에 사용됨.



---

-> 쉽게 말해, 사용자가 원하는 다양한 명령을 쉽게 표현할 수 있게 구문 약속을 해야함. 그리고, 해석자에서는 이와 같이 약속된 구문을 입력 인자로 전달되었을 때 이를 해석을 할 수 있어야 합니다. 

 

ex) "2 add 3" 과 같은 표현은 피연산자:2, 연산자: +, 피연산자:3 으로 해석될 수 있다는 것.

 사용자가 다양한 명령을 쉬운 표현 방법으로 전달할 수 있다. 하지만, 너무 많은 명령에 대한 조합에 대해 해석자 패턴을 적용한다면 정규화 과정에 들어가는 비용이 기하급수적으로 커질 수 있으므로 그렇다면 다른 방법을 사용하는게 좋음!!! 

 

ex) 정규표현식

문자열에서 어떤 패턴을 찾는 알고리즘을 매번 작성하는 것보다는 발견할 문자열을 정의하는 정규 표현식을 해석하는 알고리즘을 만드는 것이 더 낫다!! -> **정규표현식**

---



**언제 사용하는 것이 좋을까?**

 

\- 정의할 언어의 문법이 간단할 때 

( 문법이 복잡하면 관리할 클래스가 많아져 복잡해짐)

\- 성능이 중요한 문제가 되지 않을 때

---



\* 추상구문트리 (Abstarct Sytax Tree)

고급 언어를 기계어로 번역할 때 트리 구조를 통해 표현

' a+b*c+d'를 표현할 때 AST라는 트리구조를 사용해 문장정보를 나누고, 평가의 순서를 정할 수 있다.



![img](https://k.kakaocdn.net/dn/lJjfu/btquTbePFNC/U1WrHcbfxwI84noe3A89MK/img.png)

---



**결과**

**- 문법의 변경과 확장이 쉽다 : 상속 이용**

**- 문법 구현이 용이 : 노드에 해당되는 클래스들은 비슷한 구현방법을 갖기 때문에 이들 클래스를 작성하는 것이 쉬움**

**- 복잡한 문법은 관리하기 어려움 : 많은 규칙을 포함한 문법은 관리, 유지 어려움 -> 컴파일러 생성기나 파서 생성기 이용하는 것이 좋음**

---





 

 

예제) 



![img](https://k.kakaocdn.net/dn/cJlgX9/btquUBwUopF/99k0t98hbxSK9ZhkL7ALr0/img.png)

```java
package interpreter;

public interface Expression {
	public boolean interpret(String context);
}
package interpreter;

public class TerminalExpression implements Expression {
	private String data;
	public  TerminalExpression(String data) {
		this.data=data;
	}
	@Override
	public boolean interpret(String context) {
		if(context.contains(data)) return true;
		  return false;
	}

}
```

```java
package interpreter;

public class OrExpression implements Expression {
	private Expression exp1 =null;
	private Expression exp2 = null;
	
	public OrExpression(Expression exp1, Expression exp2) {
		this.exp1 = exp1;
		this.exp2 = exp2;
	}

	@Override
	public boolean interpret(String context) {
		return exp1.interpret(context)||exp2.interpret(context);
	}

}
```

```java
package interpreter;

public class AndExpression implements Expression {
	private Expression exp1 =null;
	private Expression exp2 = null;
	
	public AndExpression(Expression exp1, Expression exp2) {
		this.exp1 = exp1;
		this.exp2 = exp2;
	}

	@Override
	public boolean interpret(String context) {
		return exp1.interpret(context)&&exp2.interpret(context);
	}

}
```

```java
package interpreter;

public class InterpreterPatternDemo {
	//규칙을 생성하고 그것을 파싱하기 위해 Expression클래스를 사용한다
	
	//Rule : Robert and John are male
	public static Expression getMaleExpression() {
		Expression robert=new TerminalExpression("Robert");
		Expression john = new TerminalExpression("John");
		return new OrExpression(robert, john);
	}
	
	//Rule : Julie is a married women
	public static Expression getMarriedWomanExpression() {
		Expression Julie =new TerminalExpression("Julie");
		Expression married =new TerminalExpression("married");
		return new AndExpression(Julie,married);
	}
	
	public static void main(String[] args) {
		Expression isMale=getMaleExpression();
		Expression isMarriedWoman=getMarriedWomanExpression();
		System.out.println("John is male? "+isMale.interpret("John"));
		System.out.println("Julie is a married women? "+isMarriedWoman.interpret("Julie married"));
		
	}
}
```






Client는 문장해석의 주체이면서 인터프리터를 Context에 대해 적용

Context는 interpreter의 전역정보를 가지고 있다.

아래 그림을 보면 TerminalExpression과 NonterminalExpression이 존재한다. 트리구조를 생각하면 편하다.

표현식안에 다른 표현식(자식 노드가 있다면)에는 다시 평가를 수행해야 한다.

 



![img](https://k.kakaocdn.net/dn/bIwOwJ/btquXkOLvut/LPJasDem0fayutumt82YV0/img.png)



 

 

 

 

 

 

 

 

 

[참고]

 

[https://kunoo.tistory.com/entry/%ED%96%89%EC%9C%84-%ED%8C%A8%ED%84%B4-Interpreter-pattern-%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%84%B0-%ED%8C%A8%ED%84%B4](https://kunoo.tistory.com/entry/행위-패턴-Interpreter-pattern-인터프리터-패턴)

<https://m.blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220664898533&proxyReferer=https%3A%2F%2Fwww.google.com%2F>

 

 