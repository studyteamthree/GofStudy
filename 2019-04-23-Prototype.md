# Prototype Pattern



### What's Prototype Pattern ?

- 생성할 객체들의 타입이 프로토타입인 인스턴스로부터 결정되도록 하며, 인스턴스는 새 객체를 만들기 위해 자신을 복제(clone)하게 된다.

  

  ------

  

### Purpose

- #### 장점

  - 객체를 생성해주기 위한 별도의 객체 생성 클래스가 불필요하다.

  - 객체의 각 부분을 조합해서 생성되는 형태에도 적용가능하다.

    

- #### 단점

  - 생성될 객체들의 자료형인 클래스들이 모두 clone() 메소드를 구현해야 한다.

    ex ) 작성해야할 코드가 많아짐

    

- #### 용도

  - `prototype pattern`은 object **생성비용이 높을때** 사용한다.

    ex ) object를 생성하기위해 많은 과정이 필요한 경우

  -  비슷한 object를 **지속적으로 생성해야 할 때** 유용하게 사용한다.

    ex ) 게임상점에서 아이템을 구매할 때 다양한 종류의 아이템들을 계속 새로 생성해서 제공하면 

    ​	비효율적이기 때문에 해당 아이템을 복사하여 제공해준다.

    

    ![ê´ë ¨ ì´ë¯¸ì§](https://pbs.twimg.com/profile_images/901758637940420608/FXyRFlKQ_400x400.jpg)![ê´ë ¨ ì´ë¯¸ì§](https://pbs.twimg.com/profile_images/901758637940420608/FXyRFlKQ_400x400.jpg)

    

    - 위의 두개의 포션은 모든 조건이 똑같지만, 서로 다른객체이다. 예를 들어 한 캐릭터가 포션을 100개 가지고 있다고 생각해보자. 새로운 객체를 생성하는 것이 큰문제가 안될 수 있다. 하지만, 그런 캐릭터가 수십만개가 있다고 가정해보자. 일일이 객체를 생성하는게 효율적일까? 만들어진 객체를 복사하여 가져다가 쓰는게 효율적일까? 당연히 후자가 맞을것이다. 

    ------

    

### UML



![img](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Prototype_Pattern_ZP.svg/1920px-Prototype_Pattern_ZP.svg.png)



- 클라이언트가 Prototype 인터페이스의 clone 메소드를 호출하면 **Prototype 인터페이스**를 구현하고 있는 클래스들의 clone() 메소드를 호출하면 해당클래스의 객체 복사본을 제공한다.



------



### Example

```java
interface Prototype { // 프로토타입 인터페이스
    void setX(int x);
 
    void printX();
 
    int getX();
}
 

class PrototypeImpl implements Prototype, Cloneable { //프로토타입 구현클래스
    private int x;
 	
    public PrototypeImpl(int x) {
        setX(x);
    }
 
    @Override
    public void setX(int x) {
        this.x = x;
    }
 
    @Override
    public void printX() {
        System.out.println("Value: " + x);
    }
 
    @Override
    public int getX() {
        return x;
    }
 
    @Override
    public PrototypeImpl clone() throws CloneNotSupportedException {
        return (PrototypeImpl) super.clone();
    }
}
 
public class PrototypeTest {
    public static void main(String args[]) throws CloneNotSupportedException {
        PrototypeImpl prototype = new PrototypeImpl(1000);
 
        for (int y = 1; y < 10; y++) {
            // 만들어진 객체를 복사해서 사용 - 첫번째 장점
            Prototype tempotype = prototype.clone();
 
            // 객체를 복사하였지만 새로운 값 저장가능 - 두번째 장점
            // ex) 빨간포션은 50HP,주황포션은 150HP를 채워준다. 체력+100만 해주면 됨
            tempotype.setX(tempotype.getX() * y); 
            
            tempotype.printX();
        }
    }
}
```

### 참고

[https://donxu.tistory.com/entry/Prototype-pattern-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85-%ED%8C%A8%ED%84%B4](
