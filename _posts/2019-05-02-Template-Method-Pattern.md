---
title: "Abstract Factory Pattern"
date: 2019-04-23 20:00:00 +0900
categories: Gof Study
---

# 행동 패턴(Behavior Pattern) - 템플릿 메소드



### 행동 패턴

>  객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴이자, 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 둔다.



### 목적

>  템플릿 메소드는 각 단계의 순서는 고정하고, 중간중간 들어가는 구체적인 처리는 서브클래스가 담당하게
>
>  하는 패턴. 





### UML 다이어그램

![](https://www.dofactory.com/images/diagrams/net/template.gif)

-  AbstractClass 는 전체 알고리즘 순서를 정의하고 있는 TemplateMethod(), 그 안의 기능들을 구성하는 PrimitiveOpertaion1,2() 메소드
-  ConcreteClass 는 AbstractClass에서 정의한 메소드들을 오버라이드하여 전체 알고리즘의 부분마다 역할을 바꿀수 있게 하는 부분.



### 문제 상황

커피와 티(Tea) 를 만들때 전체적인 순서는 같으나 중간마다 해주는 역할이 다를것이다 간단한 예를 들자면



커피를 만들때는

1. 물을 끓인다. 
2. 끓는 물에 커피를 풀어넣는다
3. 컵에 담는다. 

티를 만들때는 

1. 물을 끓인다.
2. 끓는 물에 티를 풀어넣는다
3. 컵에 담는다



상황은 두개의 예시만을 들었지만 더 다양한 음료들을 제조해야 한다면 재사용성과 유지보수성은 점점 더 떨어질수 있을것이다.



Beverage.class

```java
abstract class Beverage {
    // 순서를 유지해주는 템플릿 메소드
    void prepareRecipe(){
        boilWater();
        puttingIn();
        fillingCup();
    }
    
    void boilWater(){
        // boiling water...
    }
    
    // 전체 알고리즘의 일부를 수정할 수 있게 하는 것
    abstract void puttingIn();
    
    void fillingCup(){
        // filling Cup...
    }
}
```



Coffee.class

```java
class Coffee extends Beverage{
    // override
    void puttingIn(){
        // taking coffee bag
    };
}
```



Tea.class

```java
class Tea extends Beverage{
    // override
    void puttingIn(){
        // taking tea bag
    };
}
```



### 



---

### 참고자료

-  <https://jusungpark.tistory.com/24>
-  <https://yaboong.github.io/design-pattern/2018/09/27/template-method-pattern/>

