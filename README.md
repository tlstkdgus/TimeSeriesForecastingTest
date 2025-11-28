# 비트코인 가격 예측 및 트레이딩 전략 프로젝트 📈💰

[![Open Lab Notebook in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tlstkdgus/TimeSeriesForecastingTest/blob/main/lab_notebook.ipynb)
[![Open Assignment in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tlstkdgus/TimeSeriesForecastingTest/blob/main/assignment_notebook.ipynb)

머신러닝을 활용하여 비트코인의 가격 변화 방향을 예측하고, **수익률을 극대화하는 트레이딩 전략**을 개발하는 실습 프로젝트입니다.

**학생 정보**
- 이름: 신상현
- 학번: 202001896
- 제출일: 2025/11/28

## 모델 설계 설명 ##

```
1. 모델 아키텍처:
   - GRU 구조

2. 선택 이유:
   - 파라미터 효율성: 게이트 수가 적어 학습이 빠르고 메모리 효율적임
   - 학습 속도: 적은 파라미터로 인해 학습 속도가 빠름
   - 과적합 방지: 모델 복잡도가 낮아 과적합 위험 감소

3. 트레이딩 전략:
   - 확률 기반 포지션 조절

4. 하이퍼파라미터:
   - hidden_size: 100
   - learning_rate: 0.001
   - threshold: 0.2
   - position_scaling: False

5. 예제와의 차별점:
   - GRU 모델로 변경
   - 낮은 threashold:예제(0.6)보다 공격적으로 더 많은 기회 포착
   - 비례투자: invest_ratio = prob * 0.5 -> 확률의 50%만 투자하여 리스크 관리
   - `hidden_size=100`: 예제(64)보다 56%증가 → 표현력 향상
   - `dropout=0.3`: 예제(0.2)보다 50%증가 → 과적합 방지 강화

```

## 결과 분석 및 고찰 📊

<img width="1489" height="989" alt="image" src="https://github.com/user-attachments/assets/60c159e4-d74c-4101-b984-a4bcffc11263" />

**1. 모델 성능 분석**

```
- Buy and Hold 대비 수익률: +22.50%p
- 모델 예측 정확도:
    - Train Accuracy: 63.9%
    - Validation Accuracy: 52.8%
    - 과적합 없이 적절한 일반화 성능 달성
- 주요 성공/실패 시기:
    - 성공: 2024년 11월~ 2025년 1월 강세장진입 초기 포착, 2025년 10월 ~ 11월 급등장 적극 매수
    - 실패: 일부 횡보장에서 불필요한 거래로 수수료 발생, 그러나 수익이 상쇄
```

**2. 트레이딩 전략 분석**

```
- 선택한 전략:
    - threashold=0.2(20% 이상 확신 시 거래)
    - position_scaling=False
    - invest_ratio=prob*0.5(예측 확률의 50%만 투자)
- 전략의 장단점:
    - 장점:
        -threashold 0.2로 더 많은 거래 기회 포착
        - 확률의 50%만 투자하여 리스크 관리
        - 강세장에서 공격적 수익 실현
    - 단점:
        - 높은 거래 빈도로 수수료 $171.48 발생
        - 강세장에서만 검증되어 악세장 성능 미지수
- 수수료 영향:
    - My model: $171.48 (수익의 1.15%)
    - Buy and Hold: $22.50 (수익의 0.18%)
    - 결론: 수수료 6배 이상 발생했지만 거래 수익이 충분히 상쇄
```

**3. 모델 설계**

```
- 아키텍처 선택 이유: GRU 선택 -> LSTM 대비 25% 적은 파라미터로 빠른 학습, 과적합 위험 감소
- 하이퍼파라미터 튜닝:
    - hidden_size=100(예제 64 대비 +56%): 비트코인의 복잡한 패턴 학습을 위해 증가
    - dropout=0.3(예제 0.2 대비 +50%): 과적합 방지 강화
- 예제 모델과의 차이점:
    - 아키텍처: LSTM -> GRU
    - hidden_size = 64 -> 100
    - dropout: 0.2 -> 0.3
    - threashold: 0.6 -> 0.2
    - 투자 비율: prob -> prob * 0.5
    - 결과: 47.38%
```

**4. 개선 방향**

```
- 모델의 한계점:
    - 테스트 기간이 강세장으로 악세장/횡보장 성능 미검증
    - 12회 거래로 높은 수수료 발생
    - Stop-loss 매커니즘 부재로 급락 시 대응 불가
    - 모델 예측만 사용, 기술적 지표 미활용
- 추가 실험 아이디어:
    - 앙상블: GRU + LSTM + Transformer 예측 결합
    - 기술 지표 결합: RSI, MACD와 모델 예측 조합
    - 동적 threashold: 변동성에 따라 threashold 조절
    - Stop-loss 추가: 최대 손실 10% 추가
    - 롱-숏 전략 추가: 하락 예측 시 공매도
- 실전 적용 시 고려사항:
    - 리스크 관리: 최대 손실 한도, 포지션 크기 제한
    - 거래 비용: 실제 수수료, 슬리피지, 세금
    - 시장환경: 강세/약세/횡보장 구분 필요
    - 백테스팅: 긴 기간, 다양한 시장 국면 포함 필요
    - 모니터링: 실시간 성능 추적, 주기적 재학습 필요
```
