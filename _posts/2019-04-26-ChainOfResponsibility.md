# Chain Of Responsibility
---

## 사용 목적
여러 개의 객체 중에서 어떤 것이 요구를 처리할 수 있는지를 사전에 알 수 없을 때 사용됩니다. 즉 요청 처리가 들어오게 되면 그것을 수신하는 객체가 자신이 처리 할 수 없는 경우에는 다음 객체에게 문제를 넘김으로써 최종적으로 요청을 처리 할 수 있는 객체의 의해 처리가 가능하도록 하는 패턴입니다.

---
# UML Diagram
![alt_text](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99ED853359CCA7352E647C)

* Handler : 요청을 처리하기 위한 수신자들이 가져야 할 인터페이스를 정의

* ConcreteHandler : Handler 인터페이스 구현, 각자가 요청 종류에 따라 자신이 처리 할 수 있는 부분을 구현

* Client : 맨 처음 수신자에게 처리를 요구함

---
