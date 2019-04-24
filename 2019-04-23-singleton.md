---

## **Singleton Pattern**





> **싱글톤 패턴이란?**
>
> 애플리케이션 시작시 클래스가 최초 한번만 메모리를 할당(static)하고 그 메모리에 인스턴스를 만들어 사용하는 디자인패턴
>
> ---
  
  
      
      

  
  

  
  최초 생성 후, 호출된 생성자는 최초에 생성한 객체를 반환. 하나의 인스턴스만 사용하는 패턴





> **왜 사용하는가?**
>
> - 하나의 인스턴스만 사용하기 때문에 또 생성되지 않으므로 메모리 낭비 방지 
>
> - 전역 인스턴스이기 때문에 공유 가능
>
> - 두 번째 이용부터 객체 로딩 시간이 줄어들어 성능이 좋아짐
>
> ---
>
> 

 ex) DBCP(Database Connection Pool) 처럼 공통된 객체를 여러개 생성해서 사용해야 하는 상황에서    많이 사용 (쓰레드풀, 캐시, 레지스트리 설정, 로그 기록 객체등)


http://cafe.daum.net/_c21_/bbs_search_read?grpid=19VVx&fldid=Ki0S&datanum=63




### 싱글톤 기본

```java
package singleton;

public class Singleton {
	private static Singleton instance=new Singleton();
	private Singleton() {};
	public static Singleton getInstance() {return instance;}
}
문제점 : 2개 이상의 인스턴스가 생성될 수 있다
```







> **문제점은?**
>
> - 멀티스레드환경에서 동기화처리를 안하면 인스턴스가 두개가 생성되거나 하는 경우가 있음
>
> - 싱글톤 인스턴스가 너무 많은 일을 해서 데이터를 공유시킬 경우 다른 클래스의 인스턴스 간에 결합도가 높아짐
>
> ---
>
> 



#### 동기화 문제들을 해결한 싱글톤



### Thread safe Lazy Initialization(게으른 초기화)

```java
package singleton;

public class ThreadSafeLazyInitialization {
	private static ThreadSafeLazyInitialization instance;
	private ThreadSafeLazyInitialization() {}
	public static synchronized ThreadSafeLazyInitialization getinstance() {
		if(instance==null) {
			instance=new ThreadSafeLazyInitialization();
		}return instance;
	}
}
문제점 > thread-safe하나 synchronized 특성상 큰 성능저하 발생하므로 권장하지 않음
```



### Thread safe Lazy Initialization + Double-Checked Locking

```java
package singleton;

public class ThreadSafeLazyInitialization2 {
	private static ThreadSafeLazyInitialization2 instance;
	private ThreadSafeLazyInitialization2() {}
	public static   ThreadSafeLazyInitialization2 getinstance() {
		if(instance==null) {
			synchronized (ThreadSafeLazyInitialization.class) {
				instance=new ThreadSafeLazyInitialization2();				
			}
		}return instance;
	}
}
존재 여부 확인 후 동기화 -> 성능저화 완화
```









### Initialization on demand holder idiom (holder에 의한 초기화)

가장 많이 사용하고 일반적인 싱글톤

****

클래스 안에 클래스(Holder)를 두어 JVM의 class loader 매커니즘과 class가 로드되는 시점을 이용한 방법





```java
package singleton;

public class Something {
	private Something() {}
	private static class LazyHolder{
		public static final Something INSTANCE =new Something();
	}
	public static Something getInstance() {
		return LazyHolder.INSTANCE;
	}
}
> 개발자가 직접 동기화 문제를 다루는 것이 아니라 JVM에서 클래스 초기화 되는 과정에서 보장되는 특성을 이용

싱글톤 초기화 문제를 JVM에게 떠넘기는 방식

Holder안에 선언된 instance가 static 이기 때문에  클래스 로딩시점에 한번만 호출.final을 사용해 다시 값이 할당되지 않게.
```



  
