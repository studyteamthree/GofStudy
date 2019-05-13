---
title: "TDD(Test-Driven Development)"
date: 2019-05-13 20:00:00 +0900
categories: TDD
---

# TDD(Test -Driven Development) - 1장

### What's TDD?

- **테스트 주도 개발**(Test-driven development TDD)은 매우 짧은 개발 사이클을 반복하는 [소프트웨어 개발 프로세스](https://ko.wikipedia.org/wiki/소프트웨어_개발_프로세스) 중 하나이다. 개발자는 먼저 요구사항을 검증하는 자동화된 테스트 케이스를 작성한다. 그런 후에, 그 테스트 케이스를 통과하기 위한 최소한의 코드를 생성한다. 마지막으로 작성한 코드를 표준에 맞도록 [리팩토링](https://ko.wikipedia.org/wiki/리팩토링)한다.

- Client의 입장에서 느끼는 TDD의 개념은 제품을 테스트하는 쪽에 더 가깝지만, 개발자의 기준에서 TDD 는 메소드 단위 테스트를 뜻한다.

- 개발 완료 후 테스트를 진행하는 고전적인 개발 방법에서 발생하는 여러 문제점을 해소하기 위한 프로그래밍 방법론의 하나.

### 고전적인 개발 방법의 문제점
* 특정 모듈의 개발 기간이 길어질수록 개발자의 목표의식이 흐려진다.
* 작업 분량이 늘어날수록 확인이 어려워진다
* 개발자의 집중력이 필요해진다
* 논리적인 오류를 찾기가 어렵다
* 코드의 사용 방법과 변경 이력을 개발자의 기억력에 의존하게 되는 경우가 많다.
* 테스트 케이스가 적혀 있는 엑셀 파일을 보며 매번 테스트를 실행하는 게 점점 귀찮아져서는 점차 간소화하는 항목들이 늘어난다.
* 코드 수정 시에 기존 코드의 정상 동작에 대한 보장이 어렵다
* 테스트를 해보려면 소스코드에 변경을 가하는 등, 번거로운 선행 작업이 필요할 수 있다.
* 그래서 소스 변경 시 해야 하는 회귀 테스트는 곧잘 희귀 테스트(rare test)가 되기 쉽다
* 이래저래 테스트는 개발자의 귀중한 노동력(man-month)을 적지 않게 소모한다.

### TDD 진행방식

TDD를 이용한 개발은 크게 ‘질문 → 응답 → 정제’라는 세 단계가 반복적으로 이루어진다.

![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/TDD%20%EC%88%9C%EC%84%9C.PNG?raw=true)

* 질문(Ask):테스트 작성을 통해 시스템에 질문한다. (테스트 수행 결과는 실패)

* 응답(Respond):테스트를 통과하는 코드를 작성해서 질문에 대답한다. (테스트 성공)

* 정제(Refine): 아이디어를 통합하고, 불필요한 것은 제거하고, 모호한 것은 명확히 해서 대답을 정제한다. (리팩토링)

* 반복(Repeat): 다음 질문을 통해 대화를 계속 진행한다


---



### Purpose

- #### 장점

  - 소프트웨어의 품질을 비롯한 유지보수의 편의성, 가독성, 그에 따른 소프트웨어의 비용과 안정성

  - 통합 테스트단계에서의 결함이 발생 할경우 비용이 크게 들기 때문에 단위테스트를 통해 결함을 빠른 시간에 발견할 수 있기 때문에 적은비용으로 처리가 가능하다.



- #### 단점

  -  작성해야할 코드가 많아짐

  -  그에따라 개발 시간이 늘어난다 (TDD를 하지않을 때에 비해 대략 10~30%가 늘어남)



- #### 용도

  - TDD라는 방식을 통해 얻고자 하는 최종 목적은 **잘 동작하는 깔끔한 코드** 이다.



---



### Example

```java
// 계좌가 정상적으로 생성됐는지 확인하기 위한 클래스
// Account Class의 생성여부를 확인하여 결과를 확인하는 TDD방식
public class AccountTest {
    public void testAccount(){
        Account account = new Account();
        if (account == null){
            throw new Exception("계좌생성 실패");
        }
    }

    public static void main (String[] args){

        AccountTest test = new AccountTest();
        test.testAccount(); // 테스트 케이스 실행

    }

}

// 위의 상태로 시작하면 당연히 Account 클래스가 선언되어있지 않기 때문에 오류가 난다.
// 오류를 확인하고 아래와 같이 Account 클래스를 선언한다.
public class Account {
}

// 그런다음 main 메소드가 선언되어 있는 AccountTest 클래스의 소스코드를 아래와 같이 수정한다.

public class AccountTest {
    public void testAccount(){
        Account account = new Account();
        if (account == null){
            throw new Exception("계좌생성 실패");
        }
    }

    public static void main (String[] args){

        AccountTest test = new AccountTest();
        try {
        	test.testAccount(); // 테스트 케이스 실행    
        } catch (Exception e){
            System.out.println("실패(X)")
            return; // 실패할 경우 반환
        }
        System.out.println("성공(O)")

    }
}

// try ~ catch 문을 사용하여 좀더 순조로운 코드를 작성할 수 있다. 위의 코드를 실행 해보면
// 아래와같이 결과값이 나온다

결과 성공(O)

```

### PS 엉클 밥의 TDD 원칙

- ##### You are not allowed to write any production code unless it is to make a failing unit test pass.
- ##### You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
- ##### You are not allowed to write any more production code than is sufficient to pass the one failing unit test.


---



### 참고

- 참고서적 - TDD 선택법과 도구
- https://ko.wikipedia.org/wiki/%ED%85%8C%EC%8A%A4%ED%8A%B8_%EC%A3%BC%EB%8F%84_%EA%B0%9C%EB%B0%9C
