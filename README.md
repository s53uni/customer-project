<a name="top"></a>

# 고객 데이터 기반 마케팅 분석 및 전략 제안
- 구분: 개인 프로젝트
- 기간: 2024년 1월 2일 ~ 2024년 1월 5일
<br>

<details>
  <summary>Table of Contents</summary>
  
  1. [프로젝트 개요](#프로젝트-개요)
  2. [데이터 정의](#데이터-정의)
  3. [프로젝트 과정](#프로젝트-과정)
      + [데이터 전처리](#데이터-전처리)
      + [탐색적 데이터 분석](#탐색적-데이터-분석)
      + [코호트 분석](#코호트-분석)
      + [퍼널 분석](#퍼널-분석)
      + [RFM 고객 세분화](#RFM-고객-세분화)
      + [고객 생애 가치 계산](#고객-생애-가치-계산)
  5. [결론](#결론)

</details>

## 프로젝트 개요
이 프로젝트는 고객 데이터를 분석하여 고객의 구매 행동을 이해하고, 매출과 주문 수를 증가시키기 위한 마케팅 전략을 수립하는 것을 목표로 한다.
탐색적 데이터 분석을 통해 고객의 구매 패턴을 파악하고, 코호트 분석으로 고객 이탈과 유지율을 조사하며, 퍼널 분석을 통해 구매 전환율을 확인한다. 
또한, RFM 분석과 LTV 계산을 통해 고객을 세분화하고, 이를 기반으로 맞춤형 마케팅 전략을 도입하여 마케팅 효과를 극대화하고자 한다.
<br><br>

## 데이터 정의
kaggle의 Retail-Case-Study-Data를 사용했다.
<br>
해당 데이터의 목표는 고객 데이터, 거래 내역 데이터, 제품 데이터를 종합적으로 분석하여 마케팅 전략을 최적화하고 매출을 증대시키는 것이다.
<br><br>

<p align="right"><a href="#top">⬆️TOP</a></p>

## 프로젝트 과정

### 데이터 전처리

1. 고객 생년월일과 거래일자 컬럼을 날짜 타입으로 변환
2. 변환된 생년월일 컬럼으로 나이 컬럼 생성
3. 수량이 음수인 경우를 주문 취소로 간주하여, 주문 상태를 나타내는 컬럼 생성
4. 하나의 거래 코드에 주문 완료 또는 주문 취소가 두 번 이상 발생한 경우는 이상치로 판단하여 각 상태별로 하나씩만 남기고 제거
5. 주문 취소가 발생하지 않은 주문을 주문 확정으로 정의하고, 주문 확정 여부를 나타내는 컬럼 생성
6. 거래 내역 데이터에 고객 데이터, 제품 데이터 조인
<br><br>

### 탐색적 데이터 분석
#### 구매 월별 주문건수 분포
![구매월별_주문건수_분포](https://github.com/user-attachments/assets/a57bf30c-60f1-4271-b58b-8967a28b9526)
- 1월의 주문 건수가 가장 많으며, 이는 쇼핑 시즌과 관련이 있을 가능성이 있음
<br><br>

#### 총 거래 금액 분포
![총거래금액_분포](https://github.com/user-attachments/assets/7e15816a-aac0-40db-9cf1-98c1da3e2985)
- 대부분의 거래에서 낮은 매출이 발생하는 반면, 일부 거래에서는 높은 매출이 발생
- 이는 특정 고가 제품이나 대량 구매로 인한 매출 증가일 수 있음
<br><br>

#### 제품 카테고리별 매출 분포
![제품카테고리별_매출](https://github.com/user-attachments/assets/5ef90d50-5e67-4ef0-8aa2-dc969b28ecd0)
- Books, Electronics, Home and Kitchen 카테고리에서 높은 매출을 기록하고 있음
- 이 역시 특정 고가 제품이나 대량 구매로 인한 매출 증가로 해석될 수 있음

<p align="right"><a href="#top">⬆️TOP</a></p>

#### 도시별 매출 분포
![도시별_매출분포](https://github.com/user-attachments/assets/e13f49ae-7737-409d-b54e-6cbe162a7441)
- 특정 도시(지역 코드 3, 4)에서 높은 매출을 기록하고 있음
- 지역별 맞춤형 마케팅을 통해 시장 점유율을 확대할 수 있음
<br><br>

#### 매장 유형별 매출 분포
![매장유형별_매출분포](https://github.com/user-attachments/assets/9e2af6a0-c010-4819-8529-76013d7de591)
- e-Shop에서의 주문 수가 다른 매장에 비해 두 배 이상 높게 나타남
- 이에 따라 e-Shop을 중심으로 한 집중적인 마케팅 전략이 필요할 것으로 판단됨
<br><br>

#### 일자별 매출 분포
![일자별_매출분포](https://github.com/user-attachments/assets/7038fd7a-2b0b-4cba-a4f9-79c7f3f040ef)
- 2014년 2월부터 주문이 불규칙하게 발생하기 시작함
- 데이터의 이상치이거나 특정 이슈가 발생했을 가능성이 있음
<br>
<p align="right"><a href="#top">⬆️TOP</a></p>

### 코호트 분석
![2013년_코호트분석](https://github.com/user-attachments/assets/e7585702-c4cc-43fc-9560-d9d71ef1eff2)

#### 재구매 패턴 변화 분석
- 첫 구매 후 한 달이 지난 시점(M+1) 평균 재구매율은 8.0%로 나타남
- 첫 구매 후 6개월이 지난 시점(M+6) 평균 재구매율은 9.7%로 증가함

초기에는 재구매율이 낮았으나, 시간이 지나면서 고객의 재구매 패턴이 긍정적으로 변화했다. 
이는 제품이나 서비스에 대한 고객 만족도가 높아졌거나, 효과적인 마케팅 전략의 결과일 수 있다.
<br><br>
따라서, 이러한 경향을 분석하여 장기적인 마케팅 전략을 수립하는 데 활용하면 도움이 될 것이다.
<br><br>

#### M+1 구매 유지율 분석
- 9월: 10.1%로 최고치 기록
- 10월: 5.7%로 최저치 기록
- 11월: 8.6%로 어느 정도 회복

이러한 변동의 원인을 조사하여, 유사한 변동이 다시 발생하지 않도록 예방해야 한다.
<br><br>
현재의 마케팅 전략을 재평가하고, 고객과의 소통을 통해 변동의 원인을 파악한 후, 
실시간 데이터 모니터링을 통해 문제 발생 시 신속하게 대응할 수 있는 시스템을 마련하는 등의 조치가 필요하다.
<br>
<p align="right"><a href="#top">⬆️TOP</a></p>

### 퍼널 분석
![고객퍼널_전환율및이탈률](https://github.com/user-attachments/assets/6bb8ed2a-c7de-4c62-8bdb-5b61dd883d9d)

#### 첫 번째 퍼널: 전체 주문 고객 수 > 2013 주문 고객 수
- 전체 주문 고객 중 70.0%에 해당하는 3,952명이 구매로 전환됨
- 2013년에 구매를 유도하는 마케팅 전략이 효과적으로 작용했다고 평가할 수 있음

#### 두 번째 퍼널: 2013 주문 고객 수 > 2013 주문 확정 고객 수
- 94.8%의 높은 전환율을 기록했지만, 5.2%의 고객은 주문을 취소함
- 이탈 원인을 분석하여, 주문을 확정으로 전환시키기 위한 전략이 필요함

#### 세 번째 퍼널: 2013 주문 확정 고객 수 > 2013 재구매 고객 수
- 두 번째 퍼널의 44.9%에 해당하는 1,684명이 재구매로 이어짐
- 고객 충성도가 일정 수준 유지되고 있으나, 절반 이상의 고객이 재구매로 이어지지 않았음
- 재구매율을 높이기 위해 제품 품질 향상, 할인, 멤버십 등의 프로모션을 도입하는 것이 필요함
<br>
<p align="right"><a href="#top">⬆️TOP</a></p>

### RFM 고객 세분화

- r_quartile : 가장 최근 거래 날짜가 2013년 12월이면 1(recent), 아니면 0(past)
- f_quartile : 상위 25% 이상이면 1(high), 아니면 0(low)
- m_quartile : 상위 25% 이상이면 1(high), 아니면 0(low)

<br>

| RFM 스코어 | 고객 수 | RFM 등급 |
| ------ | ------ | ------ |
| 111 | 110 | 핵심 가치 고객 |
| 011 | 280 | 핵심 충성 고객 |
| 101 | 72 | 핵심 잠재 고객 |
| 001 | 474 | 핵심 위험 고객 |
| 110 | 34 | 일반 가치 고객 |
| 010 | 103 | 일반 충성 고객 |
| 100 | 284 | 일반 잠재 고객 |
| 000 | 2390 | 일반 위험 고객 |

<br>

#### 고객 세분화 등급에 따른 액션 제안
1. 핵심 가치 고객(111)
- 가장 가치가 있는 고객으로, 구매 빈도가 높으며 금전적 가치가 높은 고객
- 관계를 지속적으로 유지하기 위해 특별한 혜택과 맞춤형 마케팅 전략 필요
2. 핵심 충성 고객(011)
- 과거 구매 빈도가 높고 많은 금액을 구매했지만 최근에 구매한 적 없는 고객
- 고객의 피드백을 통해 요구 사항 변화를 이해하고 마케팅 전략 수립이 필요
3. 핵심 잠재 고객(101)
- 최근에 많은 금액을 구매했지만 자주 구매하지는 않는 고객
- 마케팅 전략을 통해 구매 동기를 부여하여 빈도를 늘리고 핵심 가치 고객으로 전환시켜야 함
4. 핵심 위험 고객(001)
- 많은 금액을 썼지만 구매 빈도가 낮고 최근에 구매한 적 없는 고객
- 구매 주기가 긴 제품을 구매했을 가능성도 있음
- 오랫동안 구매를 하지 않은 이유를 파악한 다음 개인 맞춤형 마케팅으로 재구매 유도 필요
5. 일반 가치 고객(110)
- 최근에 자주 구매했지만 적은 금액을 구매한 고객
- 잠재적인 금전적 가치는 적지만 매우 활동적이고 충성도가 높음
- 회사를 홍보하고 평판을 확대하는 데 사용할 수 있음
6. 일반 충성 고객(010)
- 구매 빈도는 비교적 높지만 소비량이 낮고 최근에 구매하지 않은 고객
- 어느 정도 충성도는 있지만 최근 요구 사항이 바뀌었을 수 있음
- 마찬가지로 피드백을 통해 요구 사항 변화를 이해하고 마케팅 전략 수립 필요
7. 일반 잠재 고객(100)
- 첫 구매를 시도한 고객이며 아직 브랜드 인지 단계에 있음
- 자주 방문할 수 있도록 출석체크 이벤트나 포인트 제도 도입을 고려
8. 일반 위험 고객(000)
- 가장 큰 그룹(약 54.8%)이며, 구매 활동이 거의 없는 고객
- 추가적인 마케팅 비용을 지출하기 보다는 브랜드 인지도를 높이는 방식으로 접근
<br>
<p align="right"><a href="#top">⬆️TOP</a></p>

### 고객 생애 가치 계산
LTV = 총 매출 * 구매 횟수 * 고객 생애 기간

- 총 매출 : 각 고객의 총 구매 금액
- 구매 횟수 : 각 고객의 총 구매 횟수
- 고객 생애 기간 : 고객이 서비스를 이용한 기간(단위:년)

#### LTV 분포 히스토그램
![LTV_분포_히스토그램](https://github.com/user-attachments/assets/d6bef078-d69b-4c3e-8ce3-162762a8f87e)
- 대부분의 고객이 중간 이하 수준의 LTV를 가지고 있으며, 일부 고객만이 매우 높은 LTV를 보이고 있음
<br><br>

#### 총 매출과 LTV 간의 관계
![총매출_LTV_관계](https://github.com/user-attachments/assets/3dbeee4c-dac9-4dbd-adc4-27131d7fc63c)
- 총 매출이 높을수록 LTV도 높아지는 경향이 있으므로, 높은 매출을 기록하는 고객에게 집중하여 전략을 수립해야 함
<br><br>

#### 평균 LTV 분석을 통한 마케팅 우선순위 설정
| RFM 스코어 | 고객 수 | 평균 LTV | RFM 등급 | 
| ------ | ------ | ------ | ------ |
| 111 | 110 | 246928.1 | 핵심 가치 고객 |
| 011 | 280 | 204581.9 | 핵심 충성 고객 |
| 101 | 72 | 111119.3 | 핵심 잠재 고객 |
| 001 | 474 | 126810.6 | 핵심 위험 고객 |
| 110 | 34 | 151405.5 | 일반 가치 고객 |
| 010 | 103 | 101081.0 | 일반 충성 고객 |
| 100 | 284 | 95905.3 | 일반 잠재 고객 |
| 000 | 2390 | 75474.8 | 일반 위험 고객 |
<br>

1. 고객 그룹 우선순위
- 핵심 가치 고객 (111): 이 그룹은 가장 높은 LTV를 가진 고객으로, 매출에 큰 기여를 하고 있음
- 핵심 충성 고객 (011): 최근 구매는 없지만 충성도가 높은 고객으로, 관심을 기울여 관계 회복 필요
- 일반 가치 고객 (110): 활동적이고 충성도가 높은 고객으로, 회사 평판을 확대하고 홍보하는 데 중요한 역할을 할 수 있음
2. 매출 증가 가능성
- 이 그룹들은 추가 구매나 고가의 제품을 선택할 가능성이 높음
- 업셀링 및 크로스셀링 전략을 통해 매출을 극대화할 수 있음
- 이들은 다른 잠재 고객에게 긍정적인 추천을 제공할 가능성도 큼
3. 마케팅 전략 제안
- 핵심 가치 고객: 맞춤형 혜택과 멤버십 프로그램을 제공하여 충성도 강화
- 핵심 충성 고객: 리마인더 이메일, 프로모션 등의 재참여 캠페인을 실행
- 일반 가치 고객: 적극적으로 관리하여 회사를 홍보와 영향력 확보에 활용
<br>
<p align="right"><a href="#top">⬆️TOP</a></p>

## 결론
첫 구매 후 재구매율이 시간이 지남에 따라 증가하여 6개월 후 9.7%로 상승했다. 
2013년 마케팅 전략은 구매 전환율 70.0%, 주문 확정율 94.8%를 달성했지만, 재구매율은 44.9%로 낮아 추가 전략이 필요하다. 
<br><br>
RFM 분석 결과, 핵심 가치 고객(111)이 가장 높은 평균 LTV인 246,928.1원으로 매출에 크게 기여하고 있으며, 
핵심 충성 고객(011)은 평균 LTV 204,581.9원으로 높은 충성도를 보이고 있다. 
따라서, 핵심 고객에게 맞춤 혜택과 로열티 프로그램을 제공하고, 충성 고객을 대상으로 재참여 캠페인을 전개해 매출을 극대화하는 전략이 필요하다.

<p align="right"><a href="#top">⬆️TOP</a></p>