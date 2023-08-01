## 목표
- 메시지 큐의 Pub/Sub 패턴과 Producer/Consumer 패턴의 차이를 이해한다
- DB와 서버와의 통신이 가능하도록 연결한다
- 특정 상황에서 SNS, SQS로 메세지가 전달되도록 시스템을 구성한다
- SQS에 들어온 메세지를 레거시 시스템(Factory API)으로 전달하는 시스템을 구성한다
- 레거시 시스템(Factory API)의 콜백 대상이되는 리소스를 생성해 데이터베이스에 접근 할 수 있게 한다

### Step 1 : Lambda 서버(Sales API) - DB 연결

![step1](https://contents-img-jeonghun.s3.ap-northeast-2.amazonaws.com/project3/project3-project-step1.png)

> **✅ 확인 포인트: 요청시 재고 감소 로그 / 재고 0 도달 → 재고없음 로그**

### Step 2 : “재고없음” 메세지 전달 시스템 구성
![step2](https://contents-img-jeonghun.s3.ap-northeast-2.amazonaws.com/project3/project3-project-step2.png)

> **✅ 확인 포인트 : 재고가 없는 경우 stock_queue에 메세지가 들어온 것을 확인**

### Step 3 : 메세지를 레거시 시스템(Factory API)로 보내줄 Lambda 구성 및 DLQ 추가
![step3](https://contents-img-jeonghun.s3.ap-northeast-2.amazonaws.com/project3/project3-project-step3.png)

> **✅ 확인 포인트 : stock_queue에서 메세지 사라짐, stock_lambda에서 생성된 로그 확인**

### Step 4 : 데이터베이스의 재고를 증가시키는 Lambda 함수 생성

![image](https://github.com/JEONGDratini/Devops-project-3/assets/62793534/bb3be8a0-1125-46c6-bf4a-be63dc82269d)

> **✅ 확인 포인트 : 재고 없음 메세지 전송 → 일정 시간 이후 다시 요청시 재고감소 작동**

### Step 5 : 추가 시나리오에 대한 아키텍처 구성

## a. 광고 중단 요청 진행 시나리오
-   요구사항
    -   재고를 채우기 위한 과정이 진행될 때 광고 담당자에게 광고 중단 요청 내용을 담은 이메일이 전송되어야 합니다.
    -   메시지에 대한 내구성을 강화하기 위해 메시지 Queue가 사용되어야 합니다.
    -   AWS SES 서비스를 이용해서 이메일을 전송해야 합니다.

## b. VIP 고객관리 프로세스 추가 시나리오
-   요구사항
    -   100개 이상 구매가 발생 시 해당 유저의 타입이 normal에서 Vip로 변경되어야 합니다.
    -   메시지에 대한 내구성을 강화하기 위해 메시지 Queue가 사용되어야 합니다.
    -   고객관리는 별도의 데이터베이스(RDS)로 관리되고 있기 때문에 해당 데이터베이스에 접근해서 정보를 수정해야 합니다.
 
![image](https://github.com/JEONGDratini/Devops-project-3/assets/62793534/fdaaf2ca-873e-4877-95f0-493a02c352fb)


