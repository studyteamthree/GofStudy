# *Factory Method Pattern*

## *사용 목적*

객체가 생성된 직후에 어떤 작업을 수행해냐 하는 경우는 자주 나타난다.

특히 문제가 되는 경우는 객체가 이벤트를 수신하는 경우이다.

만약 객체를 이벤트 수신자로 등록하는 코드를 생성자 내부에 넣어두면 생성이 완료되기 이전에 이벤트를 받게되어 오류가 발생하는 경우가 생긴다.
이러한 문제를 해결하고자 Factory Method Pattern이 등장했다.


## *역할*

객체의 생성 후 공통으로 할 일을 수행하고,
생성되는 객체의 구체적인 타입을 감춰서

#### **어떤 클래스의 인스턴스를 만들지를 서브 클래스에서 결정한다.**

*여기서 결정한다는 것은 이 패턴을 사용 할 때 서브클래스에서 실행 중에 어떤 클래스의 인스턴스를 만들지 결정하기 때문이 아닌,*

*생산자 클래스 자체가 실제 생산 될 제품에 대한 사전 지식이 전혀 없이 만들어지기 때문이다.*

*더 정확하게 표현하자면, 사용하는 서브 클래스에 따라 생산되는 객체 인스턴스가 결정 된다.*

## *UML Diagram*

![alt text](https://t1.daumcdn.net/cfile/tistory/2405494657DB33A80F)

## *구현*

```
abstract class ShapeFactory{
  public final Shape create(Color color){
    Shape shape = createShape();
    shape.setColor(color);
    // 1. 생성 후 공통으로 할 일
    return shape;
    // 2. 구체적인 타입을 알 수 없음
  }

// protected이기 때문에 외부에 노출 안됨.
// 3. 매개 변수를 받아서 생성할 객체를 결정하도록 할 수 있음.
abstract protected Shape createShape();
}
```
##### *추상 클래스로서 상속받은 서브 클래스에게 객체 생성을 위임함.*
*여기서 createShape() 가 FactoryMethod이다.*

```
class RectangleFactory extends ShapeFactory{
  @Override
  protected Shape createShape() {
  return  new Rectangle();
  }
}

class CircleFactory extends ShapeFactory{
  @Override
  protected Shape createShape() {
  return  new Circle();
  }
}
```
##### * 추상 클래스를 상속받아 구상 객체를 생산함.*

```
interface Shape {
  public void setColor(Color color);
  public void draw();
}

class Rectangle implements Shape {
  Color color;
  public void setColor(Color color){
    this.color = color;
  }
  public void draw() {
    System.out.println("rect draw");
  }
}

class Circle implements Shape {
  Color color;
  public void setColor(Color color) {
    this.color = color;
  }

  public void draw() {
    System.out.println("circle draw");
  }
}
```
##### *Shape을 리턴 타입으로 사용하기 때문에 ShapeFactory를 사용하는 곳에서는 리턴되는 Shape의 구체적인 타입(예제에서는 Rectangle인지 Circle인지)을 알 수 없다.*
