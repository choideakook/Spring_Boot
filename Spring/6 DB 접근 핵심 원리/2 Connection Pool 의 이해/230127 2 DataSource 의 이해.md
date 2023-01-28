# DataSource 의 이해

Connection 을 얻는 방법은 JDBC 의 DriverManager 을 직접 사용하거나, Connection Pool 을 사용하는 등 다양한 방법이 존재한다.

### 📍 문제점

Connection 을 얻는 다양한 방법이 존재해 기존 방식을 다른 방식으로 변경을 할 때 application 로직을 변경해야하는 불편함이 발생한다.

## ✏️ DataSource 로 문제 해결

application 로직을 변경하지 말고 Connection 을 획득하는 방법을 추상화 해서 구체화 만 갈아끼우는 식으로 설계를 하면
어떤 방식으로 변경이 되어도 로직을 변경할 필요가 없어진다.

![s6221.png](DataSource%20%E1%84%8B%E1%85%B4%20%E1%84%8B%E1%85%B5%E1%84%92%E1%85%A2%20f88df3338fdf41598b59c1db9a093c61/s6221.png)

- java 에서는 문제를 해결하기 위해 javax.sql.DataSource 인터페이스를 제공한다.
    - DataSource 는 커넥션 획득 방법을 추상화 하는 인터페이스이다.
- DriverManager 는 Data Source 를 직접적으로 구체화 할 수 없어서 Spring 에서 DriverManager 를 구현한 DriverManager DataSource 를 제공한다.