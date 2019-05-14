# DbUnit


### DbUnit 사용 목적.

근래에 만들어지는 프로그램들 중에서 데이터베이스를 사용하지 않는 것은 드물다고 할 수 있다.

그런데 이 데이터베이스를 포함한 테스트시 데이터를 직접 Access하기 때문에 힘든 부분이 있다.

테스트 코드가 데이터베이스의 상태에 의존적으로 작성되게 된다면 깨지기 쉬운 테스트 코드가 된다는 점이다.

특히 데이터베이스의 상태를 일정하게 유지하면서 테스트를 지속적으로 수행 가능하게 만들어야 한다는 점이 매우 고통스러울 수 있다.

이럴 때 도움을 받을 수 있는 대표적인 유틸리티로 DBUnit(www.dbunit.org) 가 있다.

---

### DbUnit의 장점

DBUnit은 다음과 같은 기능을 제공하여 테스트 케이스 작성 시 도움이 된다.

* 독립적인 데이터베이스 연결을 지원한다.
 - JDBC, DataSource, JNDI 방식 지원.
* 데이터베이스의 특정 시점 상태를 쉽게 내보내거나(export) 읽어들일(import) 수 있다.
 - xml 파일이나 csv 파일 형식을 지원한다. import 계열 동작일 경우에는 엑셀 파일도 지원한다.
* 테이블이나 데이터셋을 서로 쉽게 비교할 수 있다.

보통 DbUnit은 독립적으로 사용하기보다는 JUnit 등의 테스트 프레임워크 등과 함께 사용한다. 그래서 DbUnit은 테스트 프레임워크보단 테스트 지원 라이브러리에 더 가깝다.

---

### 데이터셋

DbUnit에서 이야기하는 데이터셋(DataSet)은 데이터베이스나 그 안에 존재하는 테이블 혹은 그 일부를 xml이나 csv(comma separated value) 파일로 나타낸 모습이다.

이를테면 다음과 같은 판매자(SELLER) 테이블이 있다고 하자.
![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/dataset1.JPG?raw=true)

위 내용을 DbUnit의 데이터셋으로 만들면 다음과 같다.

![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/dataset2.JPG?raw=true)

위 XML은 DbUnit에서 제공하는 여러 데이터셋 중 가장 대표적인 형식인 FlatXmlDatqaSet 형식의 데이터셋이다. Seller 라고 표시된 요소는 DB 테이블. ID/NAME/EMAIL 등의 속성은 해당 테이블의 컬럼명을 뜻한다.
