---
title: "ProxyPattern"
date: 2019-04-29 20:00:00 +0900
categories: Gof Study
---

# 구조패턴(Structural Pattern) - 프록시 패턴



### 목적

>  다른 객체에 대한 접근을 제어하기 위한 대리자 또는 자리채움자 역할을 하는 객체를 둡니다. 프록시는 실제 객체를 대신하여 어떤 일을 대신하게 되는 것입니다. 이때 중요한것은 흐름제어만을 할 뿐 결과값을 조작하거나 변경을 해서는 안된다는 점과 큰 작업과 작은 작업중 작은 작업을 프록시가 처리하게 하는것입니다.





### 활용성

1. **원격지 프록시**
   -  서로 다른 주소 공간에 존재하는 객체를 가리키는 대표 객체.(대표적으로 JAVA의 RMI)
2. **가상 프록시**
   -  요청이 있을때만 필요한 고비용의 객체를 생성하게 함. 꼭 필요로 하는 시점까지 객체의 생성을 연기하고, 해당 객체가 생성된 것처럼 동작하게 함
3. **보호용 프록시**
   -  객체에 대한 접근 권한을 제어하거나 객체마다 접근 권한을 달리하고 싶을 때 사용하는 패턴으로 실객체에 대한 접근을 가로채어 중간에서 권한 점검을 수행하는 것.



![](https://realzero0.github.io/assets/img/proxydiagram.png)



### 참여자

1. **Proxy** : 실제 참조할 대상에 대한 참조자를 관리합니다. RealSubject와 Proxy의 인터페이스가 동일하면 프록시는 Subject에 대한 참조자를 갖습니다.
   -  원격지 프록시 : 요청 메시지와 인자를 인코딩하여 이를 다른 주소 공간에 있는 실제 대상에게 전달
   -  가상의 프록시 : 실제 대상에 대한 추가적 정보를 보유하여 실제 접근을 지연할 수 있도록 해야 함
   -  보호용 프록시 : 요청한 대상이 실제 요청할 수 있는 권한이 있는지 확인함.
2. **Subject** : RealSubject와 Proxy에 공통적인 인터페이스를 정의하여, RealSubject가 요청되는 곳에 Proxy를 사용할 수 있게 합니다.

3. **RealSubject** : 프록시의 대표격이 되는 실제 객체입니다.



### 예제 코드

## main class

실질적으로 사용되는 핵심 코드는 아래의 2가지이다. 하나는 인터페이스고 하나는 인터페이스를 상속받은 클래스이다. 이 상태에서 일반적으로 코딩을 하게 되면 무조건 CommandExcutorImpl를 호출하여 객체를 생성시키고 높은 cost의 runCommand 메소드 작업으로 인해 메모리 낭비가 예상된다.

```java
public interface CommandExecutor {
    public void runCommand(String cmd) throws Exception ;
}
public class CommandExcutorImpl implements CommandExecutor {
    @Override
    public void runCommand(String cmd) throws IOException {
        // 뭔가 높은 cost의 작업이 필요함.
        Runtime.getRuntime().exec(cmd);
        System.out.println("cmd: " + cmd);
    }
}
```

## proxy class

위의 단점을 보완하기 위에 프록시 클래스를 작성하였다. 일단 똑같은 인터페이스를 상속받음으로 인터페이스의 일관성을 유지한다. 그리고 생성자에서 CommandExcutorImpl 클래스를 인스턴화 시키고, 인스턴스화 된 executor 객체의 runCommand 메소드를 프록시 클래스의 runCommand 메소드에서 액세스를 결정한다.

```java
public class CommandExecutorProxy implements CommandExcutor {
    private boolean isAdmin;
    private CommandExecutor executor;

    public CommandExecutorProxy(String user, String pwd) {
        if ( "seotory".equals(user) && "pw".equels(pwd) ) {
            isAdmin = true;
        }
        executor = new CommandExcutorImpl();
    }

    @Override
    public void runCommand(String cmd) throws Exception {
        if (isAdmin) {
            executor.runCommand(cmd);
        } else {
            if (cmd.trim().startsWith("rm")) {
                throw new Exception("rm is only admin");
            } else {
                executor.runCommand(cmd);
            }
        }
    }
}
```

# testing

```java
public class ProxyPatternTest {
    public static void main(String[] args) {
        CommandExcutor executor = new CommandExecutorProxy("seotory", "is_not_pw");
        try {
            executor.runCommand("ls -al");
            executor.runCommand("rm -rf *"); // proxy 클래스에 의해 에러가 날 것이다.
        } catch (e) {
            System.out.println("Exception Message: " + e.getMessage());
        }
    }
}
```

## 결과 output

```java
cmd: ls -al
cmd: rm -rf *
Exception Message: rm is only admin
```



### 패턴 비교

-  **어뎁터 패턴 과 비교**	
   어뎁터 패턴은 실제 오브젝트와 다른 인터페이스를 제공하여 실제 오브젝트를 사용할 수 있도록 도와준다. 그러나 프록시 패턴은 실제 오브젝트와 동일한 인터페이스를 제공한다.
-  **데코레이터 패턴 과 비교**
   데코레이터 패턴은 런타임에 실제 객체에 동작을 추가하고 동적으로 새로운 행동들을 추가할 수 있다. 그러나 프록시는 본 객체의 동작을 직접적으로 제어하지 않고 동작을 변경하지 않는다.



### 프록시 패턴의 장점

\- 사이즈가 큰 객체가 로딩되기 전에도 proxy를 통해 참조를 할수 있다.(이미지,동영상등의 로딩에서 반응성 혹은 성능 향상 - 가상 프록시)

\- 실제 객체의 public, protected 메소드들을 숨기고 인터페이스를 통해 노출시킬수 있다. (안전성 - 보호 프록시)

\- 로컬에 있지 않고 떨어져 있는 객체를 사용할수 있다. (RMI, EJB 의 분산처리등 -  원격 프록시)

\- 원래 객체의 접근에 대해서 사전처리를 할수 있다 (객체의 레퍼런스 카운트 처리 - 스마트 프록시)

\- 원본 객체의 참조만 필요할때는 원복 객체를 사용하다가, 최대한 늦게 복사가 필요한 시점에 원본 객체를 복사하여 사용하는 방식. (Concurrent package의 CopyInWriteArrayList - 변형된 가상 프록시)



### 프록시 패턴의 단점

\- 객체를 생성할때 한단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될수 있다.

\- 프록시안에서 실제 객체 생성을 위해서 thread가 생성되고 동기화가 구현되야 하는 경우 성능이 저하되고 로직이 난해해질 수 있다.



### 참고 자료

<https://blog.seotory.com/post/2017/09/java-proxy-pattern> 

<https://jdm.kr/blog/235>



[https://happyer16.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B47-Proxy-pattern%ED%94%84%EB%A1%9D%EC%8B%9C-%ED%8C%A8%ED%84%B4](https://happyer16.tistory.com/entry/디자인패턴7-Proxy-pattern프록시-패턴)

<http://blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220655183083&redirect=Dlog&widgetTypeCall=true> : 다양한 프록시에 대한 장점과 정보

<https://blog.seotory.com/post/2017/09/java-proxy-pattern>