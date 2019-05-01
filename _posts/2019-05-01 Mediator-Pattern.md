# 행동패턴(Behavior Pattern) - 중재자 패턴



### 목적

>  상호작용을 해야 하는 개체들이 서로 복잡하게 관계를 맺고 있을 경우
>
>  프로그램간의 결합도는 강하게 하고 유연성은 떨어져 재사용성이 매우 낮아지는 경우가 있습니다.
>
>  중재자 패턴을 사용해 전체적으로 강한 결합도를 낮추고, 사용성과 유연성을 높이는데에 주 목표가 있습니다.



### UML Diagram

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F997F564D5C64A1631B1B26)



-  Mediator : Colleague 객체와 교류하는 데 필요한 인터페이스를 정의함
-  ConcreteMediator : Colleague 객체와 조화를 이뤄서 협력행동을 구현하며, 자신이 맡을 동료를 파악하고 관리함.
-  Collegue 클래스들 : 자신의 중재자 객체가 무엇인지 파악해 통신가능한 다른 Collegue Classs 들을 파악해놓습니다.



### 간략하게 나타내본 중재자 패턴

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F997CFC4D5C64A1631BA524)



 중재자 패턴에서는 서로 명령을 주고 받을 수 있는 형식이 있다고 했을 때 서로 명령을 주고 받는 부분을 중재하는 형식을 정의를 하게 됩니다. 그리고, 원래 서로 명령을 주고 받았던 개체들은 중재자 개체를 알게 하고 중재자 개체는 이들 개체를 알게 합니다. 이제 특정 개체가 명령을 내릴 필요가 있으면 중재자 개체에게 전달하기만 하면 됩니다. 중재자는 해당 명령을 자신이 알고 있는 개체들 중에 적절한 개체에게 전달만 하면 됩니다. 이처럼 중재자 패턴을 사용하면 복잡한 상호작용을 하기 위한 복잡한 관계를 단순화시킬 수 있게 됩니다.



### 예제 코드

Colleague.java

```java
package mediator;
 
public abstract class Colleague {
    IMediator mediator;
 
    public abstract void doSomething();
}
```



IMediator.java

```java
package mediator;
 
public interface IMediator {
    public void fight();
 
    public void talk();
 
    public void registerA(ColleagueA a);
 
    public void registerB(ColleagueB b);
}
```



ColleagueA.java

```java
package mediator;
 
public class ColleagueA extends Colleague {
 
    public ColleagueA(IMediator mediator) {
        this.mediator = mediator;
    }
 
    public void doSomething() {
        this.mediator.talk();
        this.mediator.registerA(this);
    }
 
}
```



ColleagueB.java

```java
package mediator;
 
public class ColleagueB extends Colleague {
 
    public ColleagueB(IMediator mediator) {
        this.mediator = mediator;
        this.mediator.registerB(this);
    }
    
    public void doSomething() {
        this.mediator.fight();
    }
 
}


출처: https://palpit.tistory.com/201 [palpit Vlog]
```



ConcreteMediator.java

```java
package mediator;
 
public class ConcreteMediator implements IMediator {
 
    ColleagueA talk;
    ColleagueB fight;
 
    public void fight() {
        System.out.println("Mediator is fighting");
    }
 
    public void talk() {
        System.out.println("Mediator is talking");
    }
 
    public void registerA(ColleagueA a) {
        talk = a;
    }
 
    public void registerB(ColleagueB b) {
        fight = b;
    }
 
}
```



MediatorMain.java

```java
package mediator;
 
public class MediatorMain {
    public static void main(String[] args) {
        IMediator mediator = new ConcreteMediator();
 
        ColleagueA talkColleague = new ColleagueA(mediator);
        ColleagueB fightColleague = new ColleagueB(mediator);
 
        talkColleague.doSomething();
        fightColleague.doSomething();
    }
}
```



### 장점

-  중재자를 통해서 다대다 관계를 일대다 관계로 만들어 주어 유지하기 쉽고, 확장하기도 쉽습니다.
-  각 객체의 행동과 상관없이 객체간의 연결 방법에만 집중할 수 있습니다.

### 단점

-  중재자 객체는 동료 객체간의 상호작용과 관련된 모든것이 정의되어 있어 너무 많은 객체들을 중재하다 보면 유지보수 비용도 늘고, 효과적인 방법이라 할 수 없다.



### 관련 패턴

-  **퍼사드 패턴**
   -  퍼사드 패턴은 서브시스템을 구성하는 객체로만 메시지가 전달되지만 서브시스템을 구성하는 객체가 퍼사드 객체에 메세지 전달을 할 수 없는 단방향인 반면 중재자 패턴은 양방향으로 전달이 가능하다.



### 참고자료

<https://palpit.tistory.com/201>

[http://ehpub.co.kr/19-%EC%A4%91%EC%9E%AC%EC%9E%90-%ED%8C%A8%ED%84%B4mediator-pattern/](http://ehpub.co.kr/19-중재자-패턴mediator-pattern/)

[https://newsdu.tistory.com/entry/GoF%EB%94%94%ED%8C%A8-17-%EC%A4%91%EC%9E%AC%EC%9E%90Mediator-%ED%8C%A8%ED%84%B4](https://newsdu.tistory.com/entry/GoF디패-17-중재자Mediator-패턴)
