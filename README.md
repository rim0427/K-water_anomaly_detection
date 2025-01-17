# K-water_anomaly_detection
2024 제4회 K-water AI 경진대회 : 상수도 관망 이상 감지 AI 알고리즘 개발


## Result
- public: 0.7647 (5th)
- private: 0.39393 (5th)
- awards: Top 0.4% out of 1,156 participants

## 주최/운영
- 주최: K-water
- 운영: 데이콘

## Task
알고리즘 | 시계열 | 정형 | 이상 탐지

## 대회 접근법

본 대회는 상수도 관망의 이상 시점과 누수 발생 구간을 정확히 탐지할 수 있는 범용 AI 알고리즘 개발하는 것이었으며,

이상 '시점'과 누수 발생 '구간'을 탐지하는 것이 핵심이었습니다.

### 제약 조건

학습 데이터는 A와 B 구조를 가진 상수도 관망 데이터로, 분 단위의 시간 정보가 모두 공개되어 있었고

반면, 평가 데이터는 C와 D 구조를 가진 상수도 관망 데이터로, 현재 시점 T 기준으로 시간이 비식별화 되어 있었습니다.

따라서 관망구조별로 압력계/유량계의 위치와 개수, 펌프의 유무 및 위치가 모두 다르기 때문에

특정 관망 구조에 유리하지 않은 범용적인 알고리즘을 개발해야 합니다.


### 문제 해결 전략

문제를 해결하기 위해 '누수' 탐지라는 것에 집중하여 가설을 설정하였고, 데이터 분석 및 시각화를 통해 검증했습니다.

#### 전략 1. 유입 유량과 유출 유량의 차이

정상 상태라면 상수도 관망에서 유입 유량과 유출 유량이 동일해야 합니다.

하지만 누수가 발생했다면, 유입 유량과 유출 유량에 차이가 있을 것이라고 가설을 설정했습니다.

실제로 관망 구조에 따라 차이를 계산하여 시각화 해보니 이상치 기준을 초과하는 시점들이 존재했고,

이는 학습 데이터에서의 이상값과 모두 부합했습니다.

#### 전략 2. 압력 감소율

또한 누수가 발생하면 특정 구간에서 압력이 급격하게 감소할 것이라고 가설을 설정했습니다.

따라서 평가 데이터의 현재 시점 T와 그 최근 시점들 중 하나라도 과거 시점과의 감소율이 설정한 임계값을 넘는다면 누수로 판단했습니다.

#### 전략 3. 유량과 압력의 관계

누수 지점을 더욱 정확하게 파악하기 위해, 해당 압력계와 가장 가까운 유량계를 활용했습니다.

데이터 분석 결과, 동일한 X축에 놓인 유량계들은 매우 유사한 트렌드를 보이는 것으로 나타났습니다.

따라서 두 개의 평행한 라인이 존재할 때, 압력 차이에 의해 누수가 발생한 라인의 유량이 줄어드는 것이 아니라, 오히려 반대 라인의 유량이 감소한다는 가설을 세웠습니다.

그리고 이를 토대로, 같은 지점에서 한쪽 라인의 유량이 감소하면 반대편 라인에서 누수가 발생한 것으로 보고 이상 상태를 탐지했습니다. 

## 느낀점
대회 초반에는 상수도 관망의 구조도가 이미지로 주어진 점과 학습 관망과 예측 관망 구조도가 다르다는 점에 따라 그래프 기반 비지도 학습도 시도해보았지만, 최종적으로는 규칙 기반의 이상탐지 알고리즘을 채택했습니다.

이상 탐지에 많이 사용되는 다양한 머신러닝 모델들을 사용해보고 싶었지만 그러지 못한 점이 아쉽고,

다시 한 번 데이터 분석의 중요성을 깨닫는 대회였습니다.
