# SOGO_Smart-city-sensor
프로젝트 팀 SOGO(Sensor Of Gyeonggi’s Overview)

## 경기도 스마트 도시데이터센서(SOGO) 입지선정
- WCRC 2023 data contest
- 기간: ‘23년 10월 ~ ‘23년 11월(2개월)
- 수상: 한국로봇교육콘텐츠협회장상 - (한국로봇교육콘텐츠협회)
- 역할: 데이터 수집, 전처리, 시각화, 발표자료 제작

- 기술 스택: Python, Machine Learning, QGIS
- 주요 내용
  - python을 활용한 데이터 전처리 및 클러스터링(군집화)와 MCLP(Max Covering Location Problem) 모델링
  - QGIS를 활용한 공간분석 - 거리벡터 및 그리드, 폴리곤 생성
  - 버퍼 생성을 통한 인근 버스정류장의 이용객수 반영, 쓰레기 처리시설 개수 반영
  - 군집 중심 좌표를 위한 거리의 역수화 및 역수화 과정에서 이상치 처리(해석의 용이성)
  - 군집 중심 좌표을 활용한 군집 선정과정


- 활용 데이터 
  - <센서 입지>
    - 경기도 cctv현황 (2023년)
    - 기상청 종관 기상 관측 데이터(AWS) (2023년) 
  - <건강 취약계층>
    - 학교 위치 데이터(초,중,고등학교) (2023년) 
  - <대기오염>
    - 폐기물 처리시설 데이터 (2023년)
    - 대기환경정보데이터 (2023년)
  - <소음 및 유동인구>
    - 버스 승하차 데이터 (2021년)
    - 경기도 지하철역별 승하차 데이터 (2023년)
    - 수도권 호선 역위치 데이터
<br>


## 1️⃣ 개요
### 1.1 데이터 전처리
1) 데이터 수집 2) 결측치/이상치 파악 3) 이상치 대체 4) 지수화
### 1.2 클러스터링
1) 최적 군집 개수 설정 2) 군집화(k-means,k-medoids,Gaussian Mixture)
### 1.3 MCLP(Max Covering Location Problem)
1) MCLP 알고리즘을 활용한 최대 커버링 지점 500개 선정
<br>

## 2️⃣ 주제 정의
사회적으로 관심이 증가하고 있는 스마트 시티를 설계, 계획하는데에 있어 과거 기존 스마트시티들의 단점을 파악, 기존의 단점을 보완한
스마트 센서 설치를 통한 스마트 시티를 구현하고자 한다. 센서는 소음,진동,대기오염,풍향 등을 측정,수집 하는 점을 고려 이러한 데이터를 활용해 우선적으로 설치가 되어야하는 중요지점을 선정한다. 최적의 설치 위치를 선정하고자 CCTV 데이터,유동인구(버스, 지하철), 대기오염(미세먼지,이산화질소,일산화탄소,오존 등),학교 (건강취약계층) 데이터 등을 활용, 군집화 과정을 통해 우선 설치가 요구되는 지점을 선정하고, MCLP(Max Covering Location Problem) 알고리즘을 통해 커버링 되는 범위가 넓은 최적의 n개(500개) 설치치점을 최종 선택한다.

CCTV데이터를 활용하는 이유는 기존에 전기가 공급되는 곳에 센서를 설치했을 때의 설치 용이성과 유지보수성을 고려함. (설치 후보군으로 설정)
<br>

## 3️⃣ 과정 

❶문제정의 ❷데이터 수집 및 가공 ❸데이터 분석 ❹군집화 ❺MCLP 알고리즘(최적화) ❻서비스 제안 및 기대효과.

### 👤역할
데이터 수집, 전처리, 시각화, 발표자료 제작
<br>

### 🧐 Main Issues
- AWS 지점 데이터와 대중교통 승하차 인원데이터, 대기오염 데이터 등을 각 후보지점에 매핑, 각 데이터들이 반영된 CCTV 지점들을 군집화를 통해 후보군을 선정하고자 함.
- 데이터의 Feature가 많고 적절한 공간분석 방법론을 설정하는데 어려움이 존재.
- 군집화를 할때 최적 군집의 개수와 클러스터링 결과 해석의 어려움이 존재.

### 🛠️ Resolved
- 3가지 군집화 모델 모두 공통된 기준인 실루엣 스코어를 활용해 최적의 군집개수를 설정.
- 군집화 결과로 나온 군집에 대해 원하는 군집 선정을 위해 Value가 높은 군집을 선정하고자 군집응집성이 아닌 군집 중심 좌표해석을 통해 군집을 선정.
- 이때 군집 중심 좌표 해석의 용이성을 위해 학교-거리, 지하철역-거리 등의 거리데이터를 역수화 하고, 각 변수의 절대값 크기가 상이하여, StandardScaler를 적용하여 변수들 간의 스케일 차이를 조정. 
- 각 변수가 군집화 모델에 동등한 기여를 할 수 있도록 설계.

- 이상치 처리: <br>
날씨데이터에서 이상치는 기상청의 물리 한계 검사기준을 활용해 이상치를 제거하였음.
그외 데이터 이상치는 IQR method를 통한 이상치 정의(정규분포가 아님을 고려) 후 이상치 대체.

- 결측치 대체: <br>
1. 관측지점별로 결측치를 스플라인 보간을 활용해 대체함(주어진 데이터 내에서 부드러운 곡선이나 고차다항식을 생성해 보간) <br>
2. IDW 보간: 인접 AWS 지점과의 거리 가중치를 활용해 역거리 가중보간을 통해 대기오염 지도 생성. <br>

<br>

### 🎯 Result
## 클러스터링(군집화) 모델부분
 k-means, k-methodis, 총 3개의 군집화 모델을 활용하였음
 3개의 모델 모두 최적의 군집 개 k개를 찾을때 동일한 조건인 실루엣 스코어를 활용해 최적개수를 찾음.
 이후 스케일링 후 군집중심좌표를 활용해 군집선정.
 3개 모델에서서 각 군집을 선정후 공통되는 후보지점을 최종 후보지로 선택.

### ⭐ Learned Lessons
입지선정을 주제로 한 프로젝트는 처음 해봤는데, 센서의 특성을 고려해 여러 종류의 데이터를 종합하여 다뤄볼 수 있는 경험이 되었고, MCLP나 QGIS 등을 처음 알게되어 적용시키면서 많은 공부가 되었습니다. <br>
또한 데이터 처리 과정과 모델링 과정에서 데이터의 특징을 고려한 방법론을 설정하기 위해 많은 시간을 투자하고 논의를 거치며, 협업의 중요성을 크게 느낄 수 있었습니다.






