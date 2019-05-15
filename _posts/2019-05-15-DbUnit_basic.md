---
title: "DbUnit의 기본 정리"
date: 2019-05-15 20:00:00 +0900
categories: TDD
---


# TDD(Test -Driven Development) - DbUnit  

### DbUnit 사용 목적.

근래에 만들어지는 프로그램들 중에서 데이터베이스를 사용하지 않는 것은 드물다고 할 수 있다.

그런데 이 데이터베이스를 포함한 테스트시 데이터를 직접 Access하기 때문에 힘든 부분이 있다.

테스트 코드가 데이터베이스의 상태에 의존적으로 작성되게 된다면 깨지기 쉬운 테스트 코드가 된다는 점이다.

특히 데이터베이스의 상태를 일정하게 유지하면서 테스트를 지속적으로 수행 가능하게 만들어야 한다는 점이 매우 고통스러울 수 있다.

이럴 때 도움을 받을 수 있는 대표적인 유틸리티로 DBUnit(www.dbunit.org) 가 있다.

---

### DbUnit의 장점

 데이터베이스의 상태를 일정하게 유지하며서 테스트를 지속적으로 수행 가능하게 만드는 데 많은 작업이 필요하며 테스트 전후의 데이터베이스 상태를 비교하는 것도 손쉬운 일은 아니다. 이같은 어려움을 개선하고자 DbUnit이 등장하게 되어 개발자들에게 많은 도움을 주었다. 대표적인 장점 세가지로는

-  독립적인 데이터베이스 연결을 지원한다.
-  데이터베이스의 특정 시점 상태를 쉽게 내보내거나 읽어들일 수 있다.
-  테이블이나 데이터셋을 서로 쉽게 비교해볼 수 있다.

DbUnit은 독립적으로 사용되기 보다 다양한 프레임워크들과 같이 사용되어지며 DbUnit을 알면 테스트 케이스 작성시 DB 관리가 매우 편리해진다.


---

### 데이터셋

DbUnit에서 이야기하는 데이터셋(DataSet)은 데이터베이스나 그 안에 존재하는 테이블 혹은 그 일부를 xml이나 csv(comma separated value) 파일로 나타낸 모습이다.

이를테면 다음과 같은 판매자(SELLER) 테이블이 있다고 하자.
![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/dataset1.JPG?raw=true)

위 내용을 DbUnit의 데이터셋으로 만들면 다음과 같다.

![alt_text](https://github.com/studyteamthree/GofStudy/blob/master/assets/img/dataset2.JPG?raw=true)

위 XML은 DbUnit에서 제공하는 여러 데이터셋 중 가장 대표적인 형식인 FlatXmlDatqaSet 형식의 데이터셋이다. Seller 라고 표시된 요소는 DB 테이블. ID/NAME/EMAIL 등의 속성은 해당 테이블의 컬럼명을 뜻한다.

---

### Example

**DbUnit을 이용한 DB 데이터 초기화**

DbUnit을 이용해서 DB 데이터를 초기화하려면 다음의 두 가지 작업만 하면 된다.

- 초기 데이터를 정의하고 있는 XML 파일을 작성.

- XML 파일로부터 데이터를 읽어와 DB를 초기화.



**초기 데이터를 정의한 XML 파일 작성**

매번 동일한 상태로 DB를 만들기 위해 사용되는 XML 파일을 만드는 방법은 간단하다. 데이터를 초기화할 테이블, 테이블 컬럼 목록, 그리고 사용할 초기 데이터를 지정해주기만 하면 된다. 다음은 XML 파일의 작성 예이다.



```xml
<?xml version="1.0" encoding="UTF-8" ?>

<dataset>
    <table name="IDREPO">
        <column>ENTITY</column>
        <column>NEXTVAL</column>
        <row>
            <value>USER</value>
            <value>1000</value>
        </row>
        <row>
            <value>DEAL</value>
            <value>1000</value>
        </row>
    </table>

    <table name="USER">
        <column>USER_ID</column>
        <column>EMAIL</column>
        <column>ENC_PASSWORD</column>
        <column>NAME</column>
        <column>EMAIL_AUTH_YN</column>
        <column>AUTH_KEY</column>
        <row>
            <value>1</value>
            <value>madvirus@madvirus.net</value>
            <value>[enc]password</value>
            <value>최범균</value>
            <value>Y</value>
            <value>testauthkey</value>
        </row>
    </table>
</dataset>
```



위 파일에서 각 태그는 다음 의미를 갖는다.

- table: 테이블을 의미한다. name 속성으로 초기화할 테이블 이름을 지정한다.

- - column: 데이터 초기화시에 사용할 컬럼 목록을 지정한다.

  - row: 한 개의 레코드를 의미한다.

  - - value: 해당하는 컬럼에 삽입될 값을 지정한다. 동일한 순서의 column 태그와 쌍을 갖는다.

예를 들어, 위 설정의 경우  IDREPO 테이블에 두 개의 레코드를 삽입하는데, 첫 번째 레코드는 ENTITY 컬럼과 NEXTVAL 컬럼의 값으로 각각 USER와 1000을 사용한다. 비슷하게 USER 테이블에도 USER_ID 컬럼 값이 1이고 EMAIL 컬럼 값이 madvirus@madvirus.net인 한 개의 레코드를 삽입한다.





**XML 파일로부터 DB 데이터 초기화하기**

알맞게 XML 파일을 작성했다면, 그 다음으로 할 작업은 이 XML 파일을 이용해서 DB를 초기화하는 것이다. 이를 위해 작성한 코드는 다음과 같다.



```java
// SeedDataLoader 클래스

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.sql.SQLException;
import org.dbunit.database.IDatabaseConnection;
import org.dbunit.dataset.DataSetException;
import org.dbunit.dataset.IDataSet;
import org.dbunit.dataset.xml.XmlDataSet;
import org.dbunit.operation.DatabaseOperation;

public class SeedDataLoader {
    public static void loadIntegrationTestDataSeed() {

        loadData("src/test/resources/inttest-seed-data.xml");

    }

    public static void loadData(String seedFile) {
        IDatabaseConnection conn = null;
        try {
            conn = DbUnitConnectionUtil.getConnection();
            IDataSet data = createDataSet(seedFile);
            DatabaseOperation.CLEAN_INSERT.execute(conn, data);
        } catch (Throwable e) {
            throw new RuntimeException(e);
        } finally {
            close(conn);
        }
    }

    private static IDataSet createDataSet(String seedFile)
            	throws DataSetException, FileNotFoundException {
        return new XmlDataSet(new FileReader(seedFile));
    }

    private static void close(IDatabaseConnection conn) {
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
            }
        }
    }
}

// DbUnitConnectionUtil 클래스
public class DbUnitConnectionUtil {
    public static IDatabaseConnection getConnection()
           		 throws ClassNotFoundException, SQLException, DatabaseUnitException {
        Class.forName(JdbcConstants.DRIVER);
        return new DatabaseConnection(DriverManager.getConnection(
                JdbcConstants.JDBCURL, JdbcConstants.USER,
                JdbcConstants.PASSWORD));
    }
}
```



위 코드에서 핵심 부분은 다음의 두 부분이다.

\- XmlDataSet을 이용해서 초기화할 데이터 집합을 생성하는 부분

\- DataOperations.CLEAN_INSERT.execute()를 이용해서 데이터를 초기화하는 부분

XmlDataSet은 앞서 작성했던 XML을 이용해서 데이터 집합을 생성할 때 사용된다. 이렇게 생성한 데이터 집합을 테스트 DB에 반영하려면 DataOperations가 제공하는 CLEAN_INSERT를 사용하면 된다. CLEAN_INSERT는 전달받은 데이터집합과 관련된 모든 테이블의 데이터를 삭제한 뒤에, 데이터를 추가한다. 이 작업을 테스트 전에 수행하면 모든 데이터를 지우고 설정된 데이터 집합을 삽입하기 때문에, 항상 동일한 상태로 테스트를 실행할 수 있게 된다.



**테스트 코드에서 사용하기**



실제 테스트 코드에서 사용하려면 다음과 같이 테스트를 수행하기 전에 DB를 초기화해주면 된다.



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = { ApplicationContextConfig.class })
public class IntTestBase {

    @BeforeClass
    public static void setup() {
        SeedDataLoader.loadIntegrationTestDataSeed();
    }
}

public class DealRepositoryIntTest extends IntTestBase {

    @Autowired
    private DealRepository dealRepository;

    @Test
    public void shouldReturnDealsWhichIdIsLessThanThreeByUsingSpecAndPageable() {
        Specification<Deal> specs = DealSpecs.beforeDeal(4L);
        Pageable pageable = createPageable();
        List<Deal> deals = dealRepository.findBySpecification(specs, pageable);
        assertEquals(2, deals.size());
        assertEquals(3L, getIdOfIdexedDeal(deals, 0));
        assertEquals(2L, getIdOfIdexedDeal(deals, 1));
    }
    ...
}
```





------







### 참고


- <https://javacan.tistory.com/entry/TIP-Init-Test-DB-Data-Using-DbUnit>
- https://repo.yona.io/doortts/blog/issue/5
- TDD 실천법과 도구
- DbUnit 2.6.0 / camel-jdbc-2.18.2
