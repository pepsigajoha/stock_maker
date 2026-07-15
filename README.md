# 📈 하이브리드 다요인 주가 시뮬레이터 (Hybrid Multi-Factor Market Simulator)

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-1.20+-red.svg)
![NumPy](https://img.shields.io/badge/NumPy-Data_Science-yellow.svg)
![Finance](https://img.shields.io/badge/Domain-Quant_Finance-green.svg)

---

## 주요 공

시뮬레이터의 최종 주가는 다음 세 가지 핵심 금융 공학 공식의 결합으로 산출됩니다.

### 1. 거시경제 동적 드리프트 (Dynamic Macro Drift)
전통적 모델의 고정된 연평균 기대 수익률($\mu$)을 동적인 거시경제 함수로 치환합니다.

$$ \mu_t = \mu_{base} + \sum (w_{s, i} \cdot \Delta x_i) + \sum (w_{g, i} \cdot (x_i - base_i)) $$

*   **뉴스 충격(Shock, $\Delta x_i$):** 전일 대비 경제 지표의 단기적 변화량입니다. (예: 금리 깜짝 인상 시 즉각적인 시장 발작 모사)
*   **구조적 중력(Gravity, $x_i - base_i$):** 현재 경제 지표가 정상 궤도(Base)에서 벗어나 있는 절대적 격차입니다. (예: 고금리가 장기화될 때 매일 주가 상승을 억누르는 만성적 압박 모사)

### 2. GARCH(1,1) 기반 레짐 스위칭 변동성
위기 국면에서의 변동성 군집(Volatility Clustering) 현상을 포착합니다.

$$ \sigma_t^2 = \omega + \alpha \cdot r_{t-1}^2 + \beta \cdot \sigma_{t-1}^2 $$

*   $\alpha \cdot r_{t-1}^2$ (ARCH 항): 전날 발생한 큰 주가 충격이 오늘의 리스크를 키웁니다.
*   $\beta \cdot \sigma_{t-1}^2$ (GARCH 항): 한 번 불안해진 시장은 지속적으로 불안한 상태를 유지하려는 관성을 가집니다.
*   **레짐 스위칭(Regime Switching):** 실업률 5% 또는 물가 8% 초과 시 $\sigma_t$를 강제로 2배 폭증시켜 금융 위기 국면으로 전환합니다.

### 3. 머튼 점프-확산 모형 (Merton Jump-Diffusion SDE)
이토의 보조정리(Ito's Lemma)를 적용한 일간 주가 업데이트 최종 이산화 공식입니다.

$$ S_{t+\Delta t} = S_t \exp\left( \left(\mu_t - \frac{\sigma_t^2}{2}\right)\Delta t + \sigma_t \sqrt{\Delta t} Z_1 + Z_2 N_t \right) $$

*   **볼래틸리티 드래그 ($-\frac{\sigma_t^2}{2}$):** 기하학적 복리 수익률 특성상 변동성이 커질수록 계좌가 녹아내리는 현상을 수학적으로 구현합니다.
*   **확산 항 ($\sigma_t \sqrt{\Delta t} Z_1$):** 숄레스키 분해를 통해 상관관계(Correlation)가 얽힌 10개 종목의 연속적인 무작위 움직임입니다.
*   **점프 항 ($Z_2 N_t$):** 포아송 분포($N_t \sim Poisson(\lambda)$)를 따르는 희소한 '블랙스완' 이벤트입니다. 시장에서 갭 하락/상승(단절)을 발생시킵니다.

---

## 🚀 설치 및 실행 방법

```bash
# 1. 필요 라이브러리 설치
pip install streamlit numpy pandas matplotlib

# 2. 로컬 웹 서버 실행
streamlit run app.py
