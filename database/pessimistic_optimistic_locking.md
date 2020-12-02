# 낙관적 잠금, 비관적 잠금
- 낙관적 잠금(optimistic locking)
- 비관적 잠금(pessimistic locking)

두 프로세스 p1과 p2가 동일한 레코드에 동시에 쓰기를 수행하는 상황을 가정해보자.

## 낙관적 잠금
- 낙관적인 잠금은 레코드를 업데이트 할때에 버전을 보고, 충돌나면 그 에러를 처리하고, 충돌나지 않으면 그냥 업데이트 한다.

  (업데이트 버전 또는 최종 변경일시를 편의상 timestamp로 표기한다.)

1. p1이 레코드에 write를 수행하기 위해 read, timestamp를 기록
1. p2가 레코드에 write를 수행하기 위해 read, timestamp를 기록
1. p1이 write를 수행하기 직전 기록한 timestamp와 현재 레코드의 timestamp가 일치하는것을 확인하고 write 수행, 이와 동시에 timestamp도 변경됨
1. p2가 write를 수행하기 직전 기록한 timestamp와 현재 레코드의 timestamp가 다르므로 error를 반환
1. 에러 처리부를 어떻게 처리하느냐에 따라 오류를 출력하거나 다시 쓰기를 시도하기위해 read를 수행


## 비관적 잠금
- 비관적인 잠금은 레코드를 갱신하는 동시에 해당 컬럼을 잠그고, 트랜잭션이 커밋되면 잠금을 푼다.

1. p1이 레코드에 write를 수행하기 위해 read, 이와 동시에 레코드를 lock
1. p2가 레코드 write를 위해 read를 시도 하지만 lock이 풀릴 때 까지 wait   
(트랜잭션은 동시에 발생해도 먼저 LOCK에 성공한 프로세스가 레코드를 선점한다.)
1. p1이 write를 완료함, 이와 동시에 레코드를 unlock
1. p2가 레코드를 read, 이와 동시에 레코드를 lock
1. p2가 write를 완료함. 이와 동시에 리코드를 unlock

## 성능 차이
- LOCK을 사용하고 Wait 하는 시간이 없기 때문에 대체로 optimistic 방식일 빠르다고 한다. 하지만 ...
