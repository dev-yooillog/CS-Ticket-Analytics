# CS Ticket Analytics
### Contact Volume KPI 모니터링 및 문의 유형 자동 분류 시스템

Customer Support 티켓 데이터를 분석하여 문의 유형별 볼륨 KPI,
반복 문의 드라이버, 우선순위 예측 모델을 구축한 end-to-end 분석 프로젝트입니다.

---

## Dataset

| 파일 | 건수 | 비고 |
|---|---|---|
| aa_dataset-tickets-multi-lang-5-2-50-version.csv | 28,580건 | 메인 분석용 |
| dataset-tickets-multi-lang-4-20k.csv | 20,000건 | 보조 검증용 |
| dataset-tickets-multi-lang3-4k.csv | 4,000건 | business_type 추가 분석 |

**주요 컬럼:** subject, body, answer, type, queue, priority, language, tag_1 ~ tag_8

---

## Project Structure
```
cs-ticket-analytics/
|-- data/
|   |-- raw/
|   |-- processed/
|       |-- tickets_clean.csv
|       |-- weekly_kpi.csv
|
|-- notebooks/
|   |-- 01_eda.ipynb
|   |-- 02_inquiry_volume_kpi.ipynb
|   |-- 03_tag_repeat_analysis.ipynb
|   |-- 04_classification_model.ipynb
|   |-- 05_priority_prediction.ipynb
|   |-- 06_weekly_report.ipynb
|
|-- outputs/
|   |-- figures/
|   |-- reports/
|       |-- weekly_kpi_sample.csv
|       |-- weekly_summary.csv
|
|-- requirements.txt
```
---

---

## 문제 상황

고객센터(CSC) 운영팀은 매주 수천 건의 티켓을 수동으로 분류하고
우선순위를 판단해야 합니다.

- 어떤 큐에 티켓이 집중되는지 실시간으로 파악하기 어려움
- 반복적으로 발생하는 문의 패턴을 정량적으로 추적하는 체계가 없음
- 우선순위 판단이 담당자 주관에 의존하여 대응 속도가 불일치

이를 해결하기 위해 28,580건의 CS 티켓 데이터를 분석하여
볼륨 KPI 모니터링, 반복 문의 드라이버 식별,
우선순위 예측 모델을 end-to-end로 구축했습니다.

---

## 왜 이 데이터인가

| 선택 이유 | 내용 |
|---|---|
| 실무 구조와 동일 | queue / priority / type / tag 컬럼이 실제 CSC 운영 구조와 일치 |
| 다국어 환경 | EN + DE 두 언어가 혼재하여 실무 복잡성 반영 |
| 규모 | 28,580건으로 통계적 유의성 확보 가능 |

---

## 분석 과정
```
데이터 정제 (01)
subject 결측 처리 / answer 결측 7건 제거 / tag 컬럼 통합
볼륨 KPI 집계 (02)
큐별 총 티켓 수 / high priority 비율 / 언어별 분포 비교
반복 문의 드라이버 분석 (03)
tag 동시 출현(co-occurrence) 분석 / 우선순위별 태그 비율 비교
문의 유형 자동 분류 (04)
TF-IDF + Logistic Regression으로 4개 유형 분류
우선순위 예측 (05)
TF-IDF + queue 피처 결합 / LR vs Random Forest 비교
주간 KPI 리포트 자동 생성 (06)
전체 지표 종합 대시보드 + CSV 저장
```

---

## 인사이트

**1. 볼륨 집중도**
> Technical Support 단일 큐가 전체 티켓의 29.2%(8,361건)를 차지
> 상위 3개 큐(Technical Support / Product Support / Customer Service)가
> 전체 볼륨의 61.9% 집중

**왜 이 지표를 봤나**
큐별 인력 배치와 대응 우선순위 결정에 직접 사용되는 지표이기 때문.
볼륨이 높은 큐에 리소스가 집중되어야 전체 응답 속도가 개선됨.

---

**2. 긴급도 리스크 큐 식별**
> Service Outages and Maintenance: high priority 비율 71.0%
> Technical Support: high priority 비율 58.6%
> General Inquiry: high priority 비율 12.8%로 가장 낮음

**왜 이 지표를 봤나**
총 건수가 많은 큐와 긴급도가 높은 큐는 다릅니다.
Service Outages는 건수(1,148건)는 적지만 71%가 high priority로
대응 실패 시 고객 경험 훼손이 가장 큰 큐

---

**3. 반복 문의 드라이버**
> IT + Tech Support 태그 조합: 14,681건으로 최다 동시 출현
> Outage / Disruption / Performance 태그가
> high priority 티켓에서 유의미하게 높은 비율

**왜 이 지표를 봤나**
반복적으로 함께 등장하는 태그 조합은
해결되지 않은 근본 문제(root cause)를 나타냅니다.
이 조합을 먼저 해결하면 전체 티켓 볼륨을 줄일 수 있습니다.

---

**4. 문의 유형 자동 분류 가능성 확인**
> Logistic Regression Accuracy 0.87
> Change / Request 유형은 거의 완벽하게 분류
> Incident - Problem 간 일부 혼동 (실무에서도 경계가 모호한 유형)

**왜 이 모델을 선택했나**
TF-IDF + Logistic Regression은 해석이 가능하고
어떤 단어가 분류에 영향을 주는지 확인할 수 있어
운영팀에 결과를 설명하기 쉽습니다.

---

## 의사결정으로 이어지는 결과

| 인사이트 | 제안 액션 |
|---|---|
| Service Outages high priority 71% | 해당 큐 전담 대응 인력 우선 배치 |
| IT + Tech Support 태그 조합 집중 | 해당 이슈 FAQ / 셀프서비스 페이지 강화 |
| Incident-Problem 혼동 | 티켓 분류 가이드라인 보완 필요 |
| 우선순위 예측 RF Accuracy 0.759 | 티켓 접수 시 자동 우선순위 태깅 가능성 확인 |

---

## 예상 임팩트

- 주간 KPI 리포트 자동화: 담당자 수작업 집계 시간 절감
- 문의 유형 자동 분류(Accuracy 0.87): 라우팅 오류 감소
- 반복 문의 드라이버 식별: 근본 원인 해결로 티켓 볼륨 감소 기대

---

## 배운 점 / 개선하고 싶은 부분

**배운 점**
- 건수(볼륨)와 긴급도(priority rate)는 별도로 봐야 한다는 것
- 태그 co-occurrence 분석이 단순 빈도 분석보다
  반복 문의 패턴 파악에 효과적임을 확인

**다음에 개선하고 싶은 점**
- 타임스탬프 데이터가 있다면 시간대별 / 요일별 볼륨 분석 추가
- 해결 시간(resolution time) 데이터와 결합하여
  우선순위별 SLA 달성률 분석 가능
- 독일어(DE) 티켓 별도 분류 모델 구축
  (현재는 영어 데이터만 모델링)

---

## Tech Stack

| 분류 | 도구 |
|---|---|
| 언어 | Python 3.10 |
| 데이터 처리 | pandas, numpy |
| 시각화 | matplotlib, seaborn |
| 머신러닝 | scikit-learn (TF-IDF, LR, Random Forest) |
| 실행 환경 | Jupyter Notebook |

---
## How to Run

```bash
# 1. 패키지 설치
pip install -r requirements.txt

# 2. 데이터 배치
data/raw/ 폴더에 CSV 파일 3개 배치

# 3. 노트북 순서대로 실행
01_eda.ipynb
02_inquiry_volume_kpi.ipynb
03_tag_repeat_analysis.ipynb
04_classification_model.ipynb
05_priority_prediction.ipynb
06_weekly_report.ipynb
```
---

## Output 

### Weekly KPI Dashboard
<img width="1715" height="1374" alt="06_weekly_dashboard" src="https://github.com/user-attachments/assets/ce3ecce7-8a08-464a-8905-1c25da92fb2a" />

### Priority Risk by Queue
<img width="960" height="720" alt="02_priority_heatmap" src="https://github.com/user-attachments/assets/b13f029d-e552-4b22-a939-85cefd4ecf19" />

### Repeat Inquiry Drivers
<img width="960" height="600" alt="05_model_comparison" src="https://github.com/user-attachments/assets/595e6ab5-3eeb-4f8a-8bf4-8b46f790f6e0" />

### Priority Prediction - Model Comparison
<img width="1200" height="720" alt="03_tag_cooccurrence" src="https://github.com/user-attachments/assets/4b186384-6238-4921-9dcd-c1252dafaf1a" />
