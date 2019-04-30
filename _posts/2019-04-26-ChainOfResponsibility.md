# Chain Of Responsibility
---

## 사용 목적
여러 개의 객체 중에서 어떤 것이 요구를 처리할 수 있는지를 사전에 알 수 없을 때 사용됩니다. 즉 요청 처리가 들어오게 되면 그것을 수신하는 객체가 자신이 처리 할 수 없는 경우에는 다음 객체에게 문제를 넘김으로써 최종적으로 요청을 처리 할 수 있는 객체의 의해 처리가 가능하도록 하는 패턴.

또한, 메시지를 보내는 객체와 이를 받아 처리하는 객체들 간의 결합도를 없애기 위함.

---
## UML Diagram
![alt_text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99ED853359CCA7352E647C)

* Handler : 요청을 처리하기 위한 수신자들이 가져야 할 인터페이스를 정의

* ConcreteHandler : Handler 인터페이스 구현, 각자가 요청 종류에 따라 자신이 처리 할 수 있는 부분을 구현

* Client : 맨 처음 수신자에게 처리를 요구함

---

## 예제
![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/ChainOfResponsibility.PNG?raw=true)

GOF에는 후속 처리자들의 연결 고리를 구현하는 방법엔 두 가지가 있다고 합니다.
* 첫째는 새로운 연결을 만드는 것(일반적으로 Handler 클래스에 정의하나 ConcreteHandler 클래스에 정의할 수도 있다.)
* 둘째는 이미 있는 연결 정보를 사용하는 것 입니다.

상기 예제는 체인의 본질을 이해하기 쉽게 하기 위해 User(사용자) 부분에 연결고리를 만드는 법으로 코드를 작성했다고 합니다.

```java
// 연쇄 책임을 처리 할 Main Handler 클래스
public abstract class Chain {
	private Chain next;
	protected String expertName;
	public final void support(Problem p){
		if (solve(p)) {
			System.out.println(expertName+ "이(가) " + p.getProblemName() +"을(를) 해결했다.");
		}else{
			if (next != null) {
				next.support(p);
			}else{
				System.out.println(p.getProblemName() + "은(는) 해결할 사람이 없다.");
			}
		}
	}
	public Chain setNext(Chain next){
		this.next = next;
		return next;
	}
	protected abstract boolean solve(Problem p);
}
```

```java
// 연쇄 책임자 중 첫 번째 수신자에게 해결을 요구하는 Client 클래스
public class Problem {
	private String problemName;
	public Problem(String name) {
		this.problemName = name;
	}
	public String getProblemName() {
		return problemName;
	}
}
```

```java
// Handler 인터페이스를 구현하여 각자가 처리할 수 있는 기능을 구상해놓은 ConcreteHandler 클래스
public class Fighter extends Chain {
	public Fighter(){
		this.expertName = "격투가";
	}
	@Override
	protected boolean solve(Problem p) {
		return p.getProblemName().contains("깡패");
	}
}

public class Hacker extends Chain { "컴퓨터" }
public class Casanova extends Chain { "여자" | "여성" }
```

```java
// Test용 메인 클래스
public class Test {
	public static void main(String[] args) {
    // 여러 요구 사항을 배열에 담아 초기화
		Problem[] problems = new Problem[6];
		problems[1] = new Problem("덩치 큰 깡패");
		problems[0] = new Problem("컴퓨터 보안장치");
		problems[2] = new Problem("까칠한 여자");
		problems[3] = new Problem("날렵한 깡패");
		problems[4] = new Problem("폭탄");
		problems[5] = new Problem("차량 필요");
    // Handler 인터페이스를 구현한 ConcreteHandler 객체 생성
		Chain fighter = new Fighter();
		Chain hacker = new Hacker();
		Chain casanova = new Casanova();

    // 책임의 순서 결정
		fighter.setNext(hacker).setNext(casanova);

    // 각각의 문제를 책임순서에 따라 해결 가능 여부 파악
		for (Problem problem : problems) {
			fighter.support(problem);
		}
	}
}
```
---
## 의문점?
상기 예제는 각각의 전문가가 한가지 일을 처리하지만,
객체당 여러가지 문제를 담당할 수도 있기에 첫 번째 책임 객체에 일반적이고, 공용의 해법을 다루는 것이 전체적인 수행 속도를 위해 좋다고 합니다.

허나, GOF 디자인 패턴의 예를 보면 첫 번째 객체는 가장 특별하고, 구체적인 해법이며, 뒤로 갈수록 일반적인 해법을 다뤄 마지막까지 해결이 안됐을 땐 가장 일반적인
해답을 줘야 한다고 합니다만

이건 그냥 책임 연쇄 패턴을 어떤 종류의 해법에 쓰냐가 포인트인것 같습니다.

---
## 참고 자료
- https://skhong.tistory.com/entry/Chain-of-Responsibility-%ED%8C%A8%ED%84%B4%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
- http://egloos.zum.com/iilii/v/3863886
- https://leetaehoon.tistory.com/64   // Handler 인터페이스 구상 클래스에 next 정의한 예제
- GOF Design Pattern
