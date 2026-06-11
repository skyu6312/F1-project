# 🏎️ F1-Tire-Degradation-Predictor(예정)
> **2025 캐나다 GP 조지 러셀(George Russell)의 우승 데이터 기반 타이어 마모도(Degradation) 분석 및 페이스 예측 모델**

본 프로젝트는 FastF1 라이브러리를 활용하여 Formula 1 레이스 텔레메트리 및 랩 데이터를 수집하고, 타이어 수명(Tyre Life)에 따른 랩타임 저하 패턴을 분석 및 예측하는 머신러닝 포트폴리오입니다. 특히 **2025 캐나다 GP에서 조지 러셀이 보여준 압도적인 타이어 관리와 우승 전략**을 데이터 분석을 통해 파악하는 것을 목표로 합니다.

---

## 📌 1. 프로젝트 개요 (Project Overview)
* **배경:** 캐나다 질 빌너브 서킷(Circuit Gilles Villeneuve)은 거친 연석과 급가속/급제동 구간이 많아 뒷타이어 마모(Rear Degradation) 관리가 승패를 결정짓는 핵심 서킷입니다.
* **목적:** 2025 캐나다 GP 우승자인 조지 러셀(RUS)과 경쟁 드라이버(예: 맥스 페르스타펜)의 데이터를 비교하여, 러셀의 타이어 관리 전략을 분석하고 이를 기반으로 한 타이어 페이스 패널티 예측 모델을 구축합니다.
* **주요 기능:**
  * FastF1 캐싱 시스템 기반 고속 F1 텔레메트리 데이터 파이프라인 구축
  * 결측치(Retire 랩), 피트스톱 랩, 세이프티 카(SC/VSC) 상황 등 아웃라이어 정제 (Data Cleaning)
  * 타이어 컴파운드, 트랙 온도, 차량 연료 소모(Fuel Weight Loss) 효과를 반영한 피처 엔지니어링
  * 타이어 마모에 따른 랩타임 저하 곡선(Degradation Curve) 예측 알고리즘 구현

---

## 🛠️ 2. 기술 스택 (Tech Stack)
* **Language:** Python (v3.10+)
* **Data Library:** FastF1, Pandas, NumPy
* **Visualization:** Matplotlib, Seaborn
* **Machine Learning:** Scikit-learn (향후 XGBoost / LightGBM 확장 예정)

---

## 📂 3. 디렉토리 구조 (Directory Structure)
```text
├── f1_cache/               # FastF1 데이터 로컬 캐시 폴더 (Git 무시 설정)
├── notebooks/
│   └── 01_eda_tire_deg.ipynb  # 데이터 추출 및 드라이버별 추세선 시각화
├── src/
│   ├── data_loader.py      # FastF1 데이터 로드 및 전처리 스크립트
│   └── model.py            # 타이어 마모도 예측 모델 빌드 스크립트
├── .gitignore              # 캐시 및 가상환경 제외 설정 파일
└── README.md               # 프로젝트 메인 설명서

'''

---

## 🎯 Key Findings & Model Performance

The Machine Learning model (Linear Regression) successfully quantifies the F1 race pace and tire degradation mechanics during the 2025 Canadian Grand Prix, achieving high predictive accuracy by decoupling vehicular physics from environmental factors.

### 📊 Model Performance Metrics
* **Predictive Accuracy (RMSE):** **`0.251 seconds`**
  * The trained AI predicts lap times with a marginal error of just a quarter of a second, proving high reliability for real-time race strategy simulations.
* **Goodness-of-Fit ($R^2$ Score):** **`82.6%`**
  * Over 82% of the variance in race pace is thoroughly explained using only three core operational features: Tire Age, Fuel Progression, and Track Temperature.

---

### 🔍 AI-Driven Engineering Insights (Feature Coefficients)

The regression coefficients accurately replicate real-world Formula 1 vehicle dynamics and Circuit Gilles Villeneuve's specific track characteristics:

#### 1. Tyre Degradation Effect (`TyreLife`: +0.0087s / lap)
* Each additional lap driven on a tire set degrades performance by a mere **0.0087 seconds**.
* **Insight:** This exceptionally low degradation rate quantitatively proves that the Montreal circuit features a low-abrasion track surface, allowing drivers to execute prolonged stints without severe grip loss.

#### 2. Fuel Burn Advantage (`FuelProgression`: -2.3418s / race)
* As the car burns through its ~110kg fuel load from lights out ($0.0$) to the checkered flag ($1.0$), the weight reduction improves lap times by up to **2.3418 seconds**.
* **Insight:** This was the dominant factor of the race. The massive performance gain from fuel burn easily overpowered the minor tire degradation (+0.0087s), explaining why overall race paces consistently quickened as the stints went deeper.

#### 3. Thermal Sensitivity (`TrackTemp`: +0.1244s / °C)
* A $1^\circ\text{C}$ increase in circuit track temperature penalizes lap times by **0.1244 seconds**.
* **Insight:** F1 tires are highly sensitive to thermal management. The AI identified that track surface heating (and subsequent tire overheating) had a significantly harsher impact on performance than pure mechanical wear.
