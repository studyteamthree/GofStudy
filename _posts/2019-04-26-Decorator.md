

# Decorator Pattern



### What's Decorator Pattern ?

-  주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙이는 패턴으로, 객체에 추가적인 요건을 동적으로 첨가하며, 기능 확장이 필요할 때 서브클래싱 대신 쓸 수 있는 유연한 대안이 될 수 있다.

  

  ------

  

### Purpose

- #### 용도

  ![img](https://t1.daumcdn.net/cfile/tistory/994A953359EADC9414)

  - 위와 같이 첨가물이 추가된 음료를 각각의 클래스로 만들면 객체수가 많아지면서 유지보수가 상당히 힘들어 지기때문에 연관되어있는 클래스들을 또다른 하나의 덩어리로 묶어서 설계한 방식입니다.

    

  - 첨가물들을 추가하는 경우의 수가 많아지고 또다른 첨가물들이 추가되는 경우를 생각하면 상당히 효율적인 패턴입니다.

    

    ------

    

### UML



![img](https://t1.daumcdn.net/cfile/tistory/9964723359EAEB5E21)

​														데코레이터 패턴의 기본 UML 다이어그램





![img](https://t1.daumcdn.net/cfile/tistory/991BFB3359EAFF8A19)

​											데코레이터 패턴으로 설계한 스타버즈 커피 UML 다이어그램





![img](https://t1.daumcdn.net/cfile/tistory/99F88C3359EB1E4B0D)

​											

​												데코레이터 패턴으로 설계된 자바의 I/O 관련 클래스들

------



### Example

```java
// 최상위 클래스인 Beverage 클래스입니다. 모든 음료의 기반이 되는 클래스이며 모든 클래스는 이 클래스의 자식 클래스가 됩니다.
public abstract class Beverage {
    
    String description = "no title"; //음료 이름
 
    public abstract int cost();
 
    public String getDescription() {
        return description;
    }
    
}
```



```java
// 모든 첨가물들이 상속 받아야 하는 CondimentDecorator 클래스 입니다. 이 클래스 역시도 Beverage 클래스를 상속합니다.

public abstract class CondimentDecorator extends Beverage {
    
    public abstract String getDescription();
 
}
```



<u>Beverage 클래스를 상속받는 음료클래스들을 작성합니다.</u>

```java
public class Americano extends Beverage {
 
    public Americano() {
        super();
        description = "아메리카노";
        // TODO Auto-generated constructor stub
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return 4000;
    }
 
}
```



```java
public class CaffeLatte extends Beverage {
 
    public CaffeLatte() {
        super();
        description = "카페라떼";
        // TODO Auto-generated constructor stub
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return 5000;
    }
 
}
```



<u>첨가물 클래스들을 작성합니다.</u>

```java
public class Cream extends CondimentDecorator {
 
    Beverage beverage;
    
        
    public Cream(Beverage beverage) {
        super();
        this.beverage = beverage;
    }
 
    @Override
    public String getDescription() {
        // TODO Auto-generated method stub
        return beverage.getDescription() + ", 크림";
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + 500;
    }
    
    
 
}
```



```java
public class Shot extends CondimentDecorator {
    
    Beverage beverage;
 
    public Shot(Beverage beverage) {
        super();
        this.beverage = beverage;
    }
 
    @Override
    public String getDescription() {
        // TODO Auto-generated method stub
        return beverage.getDescription() + ", 샷";
    }
 
    @Override
    public int cost() {
        // TODO Auto-generated method stub
        return beverage.cost() + 400;
    }
 
}
```





```java
//주문을 받을 Customer 클래스 및 만들고 main 메소드를 작성합니다
public class Customer {
 
    public static void main(String[] args) {
        
        Beverage beverage = new Americano();
        beverage = new Shot(beverage); //beverage 필드에 Amerciano 인스턴스 저장
        beverage = new Shot(beverage); //beverage 필드에 Shot 인스턴스 저장
        
        System.out.println("메뉴 : " + beverage.getDescription());
        System.out.println("가격 : " + beverage.cost());
        
        
    }
}
```





### 참고

<https://gdtbgl93.tistory.com/9>

