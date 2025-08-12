# 선착순 이벤트 시스템

<img width="910" height="415" alt="image" src="https://github.com/user-attachments/assets/3dcf8862-2ed6-49c9-8c7d-9e0d35dc9cee" />

블로그: https://velog.io/@kim138762/%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%84%A0%EC%B0%A9%EC%88%9C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%8B%9C%EC%8A%A4%ED%85%9C

### 선착순 100명에게 할인쿠폰 제공하는 이벤트의 조건
- 선착순 100명에게만 지급되어야함
- 101개 이상이 지급되면 안됨
- 순간적으로 몰리는 트래픽을 버틸 수 있어야함

## 구현시 문제점과 해결 방법
### 1. 동시성 문제 (100개 이상 발급되는 문제)
<img width="2674" height="352" alt="image" src="https://github.com/user-attachments/assets/dfe0b342-28f5-46f0-b995-479d2dae3694" />

- 해결 방법: redis의 incr사용
  - redis는 싱글 스레드라 발급 작업이라 한번에 하나의 작업만 가능

### 2. 쿠폰 발급시 지연 문제와 부하 문제
![gif gif mp4](https://github.com/user-attachments/assets/0f2a0347-09d5-4a69-b169-feb34f0a232d)


- 해결방법: Kafka로 문제 해결
  - Kafka를 통해서 쿠폰 발급 요청을 받고 컨슈머의 처리 속도가 조절하여 DB가 감당할수 있는 양 만큼 요청을 보낸다

### 3. 한명이 여러개 쿠폰 발급하는 문제
- 해결 방법: redis의 set사용
  - set은 중복 제거에 효율적임

### 4. 에러 발생시 재시도 로직
- 해결 방법: 에러 로그 찍고 나중에 배치프로그램으로 추후에 추가
