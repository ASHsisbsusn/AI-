AI Project (2025-1)
딥러닝 프로젝트

"BERT와 Chronos-Bolt를 활용한 통화정책 신뢰도 정량화 이중 파이프라인 연구"
수행 역할 : 데이터 수집, BERT 텍스트 분류 모델 학습, BOK-CI 산출 로직 구현 등

<h2>Purpose</h2>
통화정책 발표가 금융시장에 미치는 영향을 정량적으로 측정하는 문제는 기존 선형 모델로는 한계가 존재</br></br>

정형 시계열 데이터와 비정형 텍스트 데이터를 동시에 활용하는 멀티모달 분석이 필요</br></br>

→ BERT 기반 텍스트 분류와 Chronos-Bolt 기반 시계열 예측을 결합하여 통화정책 신뢰도 지수(BOK-CI)를 정량적으로 산출</br>

<h2>Method</h2>
한국은행 ECOS·통계청 KOSIS의 정형 시계열 데이터(총 21개 변수)와 통화정책 결정문 PDF를 수집</br></br>

모든 데이터를 월 단위로 리샘플링하여 동일한 시간 축에서 결합</br></br>
<BERT 텍스트 분류 파이프라인>
pdfplumber로 통화정책 결정문에서 텍스트 추출 후 BERT Multilingual Cased 모델에 입력</br>

인상 / 동결 / 인하의 3방향 분류 수행 → bert_signal로 활용</br></br>

학습 설정: 학습률 2e-5, 배치 크기 16, 에포크 10</br>

클래스 불균형 완화를 위해 금리 전환점 당월에 5배 오버샘플링 및 WeightedRandomSampler 적용</br>
<Chronos-Bolt 시계열 예측 파이프라인>
직전 24개월 데이터를 롤링 윈도우 방식으로 입력하여 KOSPI 예측값 및 신뢰구간(Q10-Q90) 산출</br></br>

실제 KOSPI가 예측 신뢰구간을 벗어나는 정도를 수치화 → 통화정책 복합 신뢰도 지수(BOK-CI) 도출</br>

<h2>Result</h2>
Policy Surprise가 급증한 시점에서 Chronos-Bolt 신뢰구간 이탈 및 BOK-CI 하락이 함께 관찰됨</br></br>
<주요 시점별 결과>
2020년 1월 글로벌 충격: Policy Surprise 0.3%p 이상 급증, KOSPI Q10 하방 신뢰구간 하회, BOK-CI LOW</br></br>

2022년 빅스텝 긴축 사이클(3·6·7·10월): Policy Surprise 최고 0.5%p, BOK-CI MID·LOW 장기 잔류</br></br>

2023년 장기 동결 구간: BOK-CI HIGH(100) 등급 안정적 유지</br></br>
bert_signal은 실제 금리 조정에 앞서 선행 지표로 기능함이 확인됨</br>

2019년 인하 사이클에서는 실제 조정 수개월 전부터 완화적 신호가 선제적으로 검출됨</br>
