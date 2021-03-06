## 대규모 데이터 처리 입문

### 대규모 데이터 처리의 어려운 점

-	데이터 용량이 너무 커서 메모리 내에서 계산할 수 없을때
- 메모리에 올리지 않으면 기본적으로 디스크를 계속 읽어가면서 검색하게 되어 좀처럼 발견할 수 없는 상태가 됨.
- 제일 문제는 디스크를 읽고 있다는 점


```
메모리는 디스크보다 10만 ~ 100만배 빠르다

- 탐색속도

디스크상에 위치한다면 : 탐색할 때 헤드의 이동이나 원반의 회전과 같은 물리적인 이동이 필요
메모리상에 위치한다면 : 탐색할 때 물리적인 동작 없음

단순히 탐색속도만 빠른 것이 아니다

- 전송속도

메모리나 디스크 모두 CPU와 버스로 연결되어있다. 이 버스의 속도에서도 상당한 차이를 보임
메모리 와 CPU   : 상당히 빠른 버스



```
참고 그림

![hardware](../image/hardware.jpg)

### 규모조정의 요소
- scale-out이 scale-up보다 웹서비스에 적합
   - 비용이 저렴
   -  시스템 구성의 유연성

### 웹 어플리케이션에서의 부하

- Cpu 부하가 일어나는 요소 : http 요청받고 질의후 응답 반환하는 작업
-  DB 서버 측면에서는 I/O 부하 걸림




![book_image](../image/book_image.png)


2’ 2’’ CPU 부하에 의한 확장은 그냥 대수만 늘리기만 하면 가능 (새로운 서버 추가해서 복사본 마련)

하지만 3’ 3’’ 같이 DB I/O 부하에 의한 확장은  읽기는 문제 없지만 데이터 동기화 문제 때문에 쓸 때가 복잡하고 문제임

DB는 보통 디스크를 많이 씀 ex) 은행 전산-> 메모리에 두면 서비스 내려가면 큰일

```
AP서버란? (어플리케이션 서버)
동적 콘텐츠를 반환하는 서버를 말함.

ex) Apache + mod_perl이 동작하는 웹서버나 Tomcat과 같은 애플리케이션 컨테이너가 동작하는 서버

DB로부터 얻은 데이터를 가공해서 클라이언트로 전달하는 처리를 수행
````


##### Load Average가 의미하는 부하란?
- 처리를 실행하려고 해도 실행할 수 없어서 대기하고 있는 프로세스의 수
- cpu의 실행권한이 부여되기는 바라는 프로세스, 디스크 I/O가 완료하기를 기다리고 있는 프로세스) 두개 다 이기 때문에 load average 가지고는 cpu부하가 높은지 I/O 부하가 높은지 모른다

##### 통합적인 조사 필요
load average는 Cpu 부하인지 I/O 부하인지 잘 모른다고 했다
Sar을 이용하면 CPU와 I/O 중 어느쪽에 원인이 있는지 조사 가능하다
종합적인 수치 -> 구체적인 수치순으로 따지기


### 대규모 데이터를 다루기 위한 기초지식
<br>

#### 대규모 데이터 다루는 요령
1.	어캐하면 메모리에서 처리를 마치고 디스크 seek을 최소화할까?
2.	데이터량 증가에 강한 알고리즘 사용 ( 선형검색 ->  이분검색)
3.	데이터 압축 단적으로 데이터량을 줄일 수 있다면 당연히 디스크 읽는 횟수도 최소화 가능 , 검색 테크닉 ex) 검색 엔진 제작

#### 대규모 데이터를 다루기 전 3대 지식

1.	OS 캐시
2.	분산 고려 RDBMS 운용할 때는 어떻게?
3.	대규모 환경에서 알고리즘, 데이터 구조 사용은 어떤 것?
