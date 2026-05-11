## Buldak Global Trend & Export Analysis

데이터 기반 불닭 시리즈 글로벌 시장 성과 분석 및 향후 6개월 수출 예측

---
## Problem Statement

프로젝트 기간: 2026.04.15 ~ 2026.04.30 (약 2주)

목적: 구글 트렌드 검색 데이터와 국가별 수출 통계 데이터를 결합하여 불닭 시리즈의 글로벌 흥행 메커니즘을 분석하고, 향후 수출 성과를 예측하여 마케팅 우선순위 국가 도출

Problem Statement:
국가별로 상이한 검색량-수출량 간의 시차(Time-lag)와 전환 효율(Efficiency)을 파악하고, 바이럴 이벤트가 실제 매출 증대에 미치는 영향(Lift)을 정량화하여 데이터 기반의 글로벌 확장 전략 수립

---
Tech Stack

   - Language: Python 3.10
   - Data Science: Pandas, NumPy, Statsmodels, Prophet (시계열 예측)
   - Visualization: Matplotlib, Seaborn, Squarify (트리맵)
   - Data Engineering: Scikit-learn (K-Means Clustering)

---
## Project Structure

buldak-global-analysis/
├── data/
│   ├── 01_raw/                 
│   └── 02_interim/
├── notebooks/
├── outputs/
│   ├── buldak_clean.csv            # 전처리 완료된 시계열 데이터
│   ├── buldak_enriched.csv         # 파생변수(단가, 효율 등) 포함 데이터
│   ├── cluster_result.csv          # 국가별 클러스터링 결과
│   └── forecast_6m.csv             # 6개월 예측 모델 결과값
├── src/
├── Analysis_Report.md              
└── README.md

---
## Analysis Pipeline

   - Step 1: Data Wrangling: 관세청 수출입 실적 원본 데이터를 품목별/국가별로 통합 및 전처리
   - Step 2: Enrichment: 국가별 검색 지수와 결합하여 Search-to-Export Efficiency 지표 산출
   - Step 3: EDA: 주요 지표 간 상관관계 분석, Time-Lag 시차 분석 및 수출 단가 추이 파악
   - Step 4: Advanced Analysis: K-Means 클러스터링을 통한 시장 세분화 및 바이럴 이벤트 Lift 분석
   - Step 5: Modeling: Facebook Prophet 라이브러리를 활용한 향후 6개월 수출 금액 예측

---
## Business Insights

- 고효율 팬덤형 시장 (미국, 일본): 검색지수 대비 실제 구매 전환이 매우 높음. 브랜드 로열티 강화 전략 유효.  

- 선행 지표 활용 (필리핀, 러시아): 검색량 변화가 약 3개월 뒤 수출 지표에 반영됨. 구글 트렌드 급증 시 선제적 유통망 점검 필요.  

- 고단가 전략 (영국, 미국): 톤당 수출 단가가 $5,000 이상인 고부가가치 시장으로, 프리미엄 포지셔닝 유지 필요.  

- 리스크 관리 (호주): 바이럴 이벤트 후 수출액이 감소(-18.6%)한 특이 사례로, 현지 시장의 부정적 피드백이나 일회성 요인 조사 필요.



