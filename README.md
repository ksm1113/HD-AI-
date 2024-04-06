# HD현대-AI Challenge
본 페이지는 HD한국조선해양 AI Center에서 주관하는 DACON에서 열린 HD현대 AI Challenge의 수행 과정을 다뤘습니다.
***
**[배경]**    
>조선해양 분야 데이터를 기반으로 한 'HD현대 AI Challenge'를 개최됩니다.
코로나19 이후 물류 정체로 인해 다수의 항만에서 선박 대기 시간이 길어지고, 이로 인한 물류 지연이 화두가 되고 있습니다. 
특히 전 세계 물동량의 85%를 차지하는 해운 물류 분야에서 항만 정체는 큰 문제로 인식되고 있는 상황입니다. 
본 대회에서는 접안(배를 육지에 대는 것;Berthing) 전에 선박이 해상에 정박(해상에 닻을 바다 밑바닥에 내려놓고 운항을 멈추는 것;Anchorage)하는 시간을 대기시간으로 정의하고, 선박의 제원 및 운항 정보를 활용하여 산출된 항차(voyage; 선박의 여정) 데이터를 활용하여 항만 內 선박의 대기 시간을 예측하는 AI 알고리즘을 개발을 제안합니다.
이를 통해 선박의 접안 시간 예측이 가능해지고, 선박의 대기시간을 줄임으로써 연료 절감 및 온실가스 감축효과를 기대할 수 있습니다.

**[주제]**

항만 內 선박 대기 시간 예측을 위한 선박 항차 데이터 분석 AI 알고리즘 개발

**[평가 방식]**    
*MAE*(Mean Absoulte Error,평균 절대 오차)

**[활용 데이터]**    
약 40만개의 선박의 제원 및 운항정보
| 열 | 의미 | 단위 |
|---|:---:|---:|
| `ARI_CO` | 도착항의 소속국가(도착항 앞 2글자) |  |
| `ARI_PO` | 도착항의 항구명(도착항 뒤 글자) |  |
| `SHIP_TYPE_CATEGORY` | 선종 5대 선종으로 분류 |  |
| `DIST` | 정박지(ber_port)와 접안지 사이의 거리 | `km` |
| `ATA` | anc_port에 도착한 시점의 utc. 실제 정박 시각(Actual Time of Arrival)	 |  |
| `ID` |    선박식별 일련번호 |  |
| `BREADTH`  | 선박의 폭   | `m` |
| `BUILT`|	선박의 연령	year | |
|`DEADWEIGHT` |	선박의 재화중량톤수 |	`ton`|
|`DEPTH`|선박의 깊이|`m`|
|`DRAUGHT`|	흘수 높이|	`m`|
|`GT`|	용적톤수(Gross Tonnage)값|	`GT(m^3)`|
|`LENGTH`|	선박의 길이|	`m`|
|`SHIPMANAGER`|	선박 소유주|	|
|`FLAG`|	선박의 국적	|
|`U_WIND`|	풍향 u벡터	|`m/s`|
|`V_WIND`|	풍향 v벡터	|`m/s`|
|`AIR_TEMPERATURE`|	기온	|`ºC`|
|`BN`|	보퍼트 풍력 계급|	
|`ATA_LT`|	anc_port에 도착한 시점의 현지 정박 시각(Local Time of Arrival)| `H`|
|`PORT_SIZE`|	접안지 폴리곤 영역의 크기|	`km^2`|
|`CI_HOUR`|	대기시간	|`hour`|
<br>

* * * 

**[EDA]**

# 데이터 전처리

ATA feature 데이터를 datetime으로 변환 후 hour와 weekday로 변수 추출(연관성 고려) -> 나머지 값 버림

(hour와 weekday에 대한 대기시간 그래프)

ATA, ID, SHIPMANAGER, FLAG 컬럼 버림(연관성 부족 판단)

ATA, ID, SHIPMANAGER, FLAG에 대한 대기시간 그래프

[Label encoding, One-hot encoding]

코딩 상에서는 문자가 아닌 수치상으로 인식 필요 -> 문자 데이터 Label encoding을 통해 수치형 데이터로 변환

Train 데이터에 존재하지 않는 값이 Test 데이터에 존재할 가능성을 고려하여 -1 값 삽입

-> 새로운 데이터 존재 시 -1 값 할당

범주형 데이터(SHIP_TYPE_CATEGORY) 처리를 위한 One-hot encoding 적용

배의 수치데이터 중 결측치가 존재하는 행(LENGTH) 제거

BUILT, U_WIND, V_WIND 컬럼 버림(BUILT 컬럼은 연관성 부족 판단, U_WIND 및 V_WIND의 경우 BN으로 대체)

**결측치 처리**

# 선박 대기 시간 추정을 위한 데이터 논문 발췌 내용 정리

**컨테이너선**

선석의 총길이 100m 증가 -> 평균 대기시간 약 10% 감소

선석의 평균 길이 100m 증가 -> 평균 대기시간 약 70% 감소

출처 - 선박대기시간 예측모형 개발을 위한 방법론 연구 (A Study on the Methodology for Estimating Vessel Waiting Time)

선박의 총톤수 1000톤 증가 -> 선박 재항 시간 약 2.4% 증가

동일 톤급 선박 동일 처리능력 부두 접안 시 일반화물선, 산물선 등의 화물운송선박의 재항시간이 풀컨테이너선의 재항시간보다 최대 75% 큰 값을 보임.

출처 - 생존분석모형을 이용한 선박의 재항시간 및 온실가스 배출량 분석 (Analyzing Time in Port and Greenhouse Gas Emissions of Vessels using Duration Model)

날씨가 좋을 경우 선적 속도가 빨라지고 일조량이 늘어나기 때문에 선박 대기 시간 감소.

날씨가 좋지 않은 경우 화물의 선적 및 하역이 중지되어 선박의 대기 시간 증가.

선박의 도착시간이 대기시간에 영향을 미칠 수 있음.

오전에 접안하는 선박의 경우 야간에 하역활동 빠르게 진행 가능, 야간에 접안하는 경우 노동자들의 피로와 어두운 상황으로 인해 업무수행이 느려져 대기시간 증가.

출처 - ANALYSIS OF THE INFLUENCE OF SHIP SCHEDULING, GUIDELINE SERVICES, WEATHER, AND LOADING EQUIPMENT READINESS ON SHIP WAITING TIME IN PORT

