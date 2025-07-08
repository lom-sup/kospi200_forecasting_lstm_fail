# LSTM을 이용한 KOSPI200 지수 예측 모델(*Fail*)

<br/>

## 프로젝트 개요

<br/>

> 이 프로젝트는 다양한 거시경제 지표를 바탕으로 KOSPI200 지수를 예측하는 **시계열 딥러닝 모델(LSTM)** 구현을 목표로 한다.  
> 공공 데이터(최저임금, 기준금리, 소비자물가 등)를 수집하여 월간 시계열 데이터로 구성하고,  
> Tensorflow 기반의 **다층 LSTM 모델**을 통해 **2005년부터 2022년까지의 경제 흐름을 반영한 주가지수 예측 실험**을 수행하였다.


<br/>

- **주요 목표**: 다양한 국내 경제지표를 기반으로 KOSPI200 지수를 월 단위로 예측
- **사용 모델**: PyTorch 기반 LSTM (3층 구조)
- **프로젝트 기간**: 2023년 2학기 정보통계학 수업 프로젝트
- **데이터 출처**:
  - e-나라지표 (https://www.index.go.kr/)
  - KOSIS 국가통계포털
  - 한국은행 경제통계시스템

<br/>

---

## 사용된 경제 지표

<br/>

| Feature                 | 설명                              |
|------------------------|-----------------------------------|
| minimum_wage           | 최저임금 (연 단위 → 월 단위 보정) |
| base_interest_rate     | 기준금리                          |
| exchange_rate          | 원/달러 환율                      |
| trade_balance          | 무역수지                          |
| consumer_price_index   | 소비자물가지수                    |
| employment_index       | 고용지수 (분기 → 월 변환)         |
| oil_price              | 국제 유가                         |
| gdp_growth             | GDP 성장률 (분기 → 월 변환)       |
| kospi200               | **KOSPI200 지수 (예측 대상)**     |

<br/>

---

## 데이터 전처리 및 시계열 구성

<br/>

- datetime 인덱싱 및 정렬
- 문자열/숫자 단위 처리 및 float 변환
- 월 단위 기준으로 시계열 정렬
- 결측값 처리 및 단위 맞춤 (분기 데이터를 월 단위로 보간)
- `MinMaxScaler`를 통한 정규화
- `sequence_length = 24` 기준으로 슬라이딩 윈도우 생성

<br/>

---

## 모델 구성 (PyTorch LSTM)

<br/>

```python
class KospiLSTM(nn.Module):
    def __init__(self, input_size, hidden_size, num_layers, dropout):
        ...
```

- 입력 차원: 8개 지표
- 시계열 길이: 24개월 (2년)
- LSTM 층: 3층
- 활성화 함수: tanh
- 드롭아웃: 0.1
- 출력층: Dense(Linear) 1개

<br/>


## 모델 학습 및 결과

<br/>

- 손실 함수: MSE
- Optimizer: Adam
- 학습 중 shape mismatch 오류 발생 → 데이터셋 reshape 진행
- validation loss 기준 EarlyStopping 실험


<br/>


## 평가 및 한계

<br/>

- 입력 차원과 출력 시점 설정의 오류가 예측력 저하로 이어짐
- 일부 경제지표가 분기 단위 → 월간 변환 시 정보 손실


<br/>


## 개선 방향

<br/>

- 입력차원 재설계하여 LSTM 모델 완성시키는 것이 우선순위
- 경제지표 확대 (10개 이상), 더 긴 기간 확보
- 비교 모델 추가 (ARIMA, XGBoost 등)
- attention 기반 LSTM 모델 도입 검토

<br/>


## 파일 명세

<br/>

|파일명 | 설명 | 
|--------|--------|
| kospi200_prediction_model.ipynb	| 전체 모델링 코드, 데이터 전처리 및 결과 포함 |

<br/>
