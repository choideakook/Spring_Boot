# Transaction 동기화

Spring 이 제공하는 Transaction Manager 는 크게 2가지 역할을 한다.

1. Transaction 추상화
    - 🔗 Transaction 추상화
2. 리소스 동기화
    - Transaction 을 유지하려면 처음부터 끝날 때 까지 같은 Connection 을 유지해야 한다.
    - Connection 을 유지하기위한 작업을 동기화라고 하며 Param 으로 전달하는 동기화는 코드가 너무 지저분해지는걸 포함해 다양한 문제점을 갖고 있다.
    - [🔗 Parameter 동기화의 문제점](https://github.com/choideakook/TIL/blob/main/Spring/6%20DB%20접근%20핵심%20원리/4%20Transaction%20활용/230129%201%20Transaction%20의%20문제점.md)

<br>

## ✏️ Transaction Manager 와 Transaction 동기화 Manager

🔗 예제로 확인하는 Transaction Manager 가 사용되는 과정

![s6431.png](Transaction%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%80%E1%85%B5%E1%84%92%E1%85%AA%2034f2a2c1df6a4470a696227b849daa51/s6431.png)

1. Transaction 이 시작되면 interface 인 Transaction Manager 를 호출
2. Transaction Manager 는 Data Source 를 통해 Connection 을 생성한다.
    - Connection 을 생성하기 위해서는 Data Source (DB 드라이버) 가 필요하다.
3. Transaction Manager 는 획득한 Con 을 Transaction 동기화 Manager 에 보관한다.
4. Transaction 동기화 Manager 는 ThreadLocal 이라는 기술을 사용해 획득한 Connection 을 안전하게 동기화 (보관) 해준다.
    - ThreadLocal 은 멀티 Thread 상황에서도 Connection 을 안전하게 동기화 (보관) 할 수 있다.
5. Repository 는 동기화 Manager 에 보관된 Connection 을 사용한다.
    - Connection 을 Parameter 로 넘겨주지 않아도 됨
6. Transaction 이 종료되면 Transaction Manager 는 동기화 Manager 에 보관된 Con 을 통해 Transaction 을 종료한다.

🔗 Thread Local