# aa-segment-manager 📡

Adobe Analytics API(`aanalytics2`)를 활용하여 세그먼트 간의 의존성을 분석하고 정합성을 모니터링하는 자동화 엔진입니다. 멀티스레딩을 통해 대량의 세그먼트 데이터를 빠르게 적재하고 무결성을 점검합니다.

## ✨ 주요 기능
- **멀티스레딩 데이터 적재**: `ThreadPoolExecutor`를 활용하여 수백 개의 세그먼트 데이터를 병렬로 신속하게 다운로드합니다.
- **의존성 트리 구축**: 세그먼트 정의(`definition`)를 재귀적으로 분석하여 부모-자식 세그먼트 간의 연관 관계를 매핑합니다.
- **정합성 및 영향도 분석**: 자식 세그먼트의 로직이 변경되었을 때, 이를 포함하는 부모 세그먼트 내부 로직과의 유사도(Similarity Score)를 계산하여 불일치(Mismatch)를 찾아냅니다.

## 📁 폴더 구조
```text
aa-segment-manager/
│
├── .gitignore                  # 인증 파일(JSON) 업로드 차단 설정
├── README.md                   # 본 설명서
└── aa_segment_manager/
    ├── main.py                 # 엔진 실행 및 동기화 제어 메인 스크립트
    └── aanalyticsact_auth.json # [보안] Adobe API 자격 증명 및 환경 설정 파일
🚀 시작하기
1. 필수 라이브러리 설치
본 패키지는 aanalytics2와 데이터 처리를 위한 pandas를 사용합니다.

Bash
pip install aanalytics2 pandas
2. 인증 파일 설정
aa_segment_manager/ 폴더 내에 aanalyticsact_auth.json 파일을 배치해야 합니다. 이 파일에는 API 자격 증명 정보와 대상 REPORT_SUITE_ID가 포함되어 있어야 하며, 보안을 위해 Git 업로드 대상에서 자동으로 제외됩니다.

3. 실행 방법
Bash
python -m aa_segment_manager.main
🔒 본 저장소는 민감한 자격 증명 및 고객사 식별 정보가 공개되지 않도록 보호되어 있습니다.


---

## 2️⃣ `app-traffic-analytics`용 README.md

```markdown
# app-traffic-analytics 📊

모바일 앱의 원천 트래픽 데이터를 정제하고, 비즈니스 핵심 퍼널(Funnel) 성과를 한눈에 볼 수 있도록 일별 마트(Daily Mart)를 구축하는 SQL 쿼리 저장소입니다.

## 🛠️ 주요 파이프라인 구조

1. **기간 파라미터 제어 (`params`)**
   - 임시 테이블을 통해 분석 대상 시작일과 종료일을 유연하게 지정합니다.
2. **장바구니 시퀀스 정제 (`in_view_cart`)**
   - 상품 상세, 장바구니 담기 등의 이벤트를 언네스팅(Unnesting)하여 유저 행동 흐름을 추적합니다.
3. **유저 식별 및 가속화 단계 (`raw_with_session`)**
   - 앱 인스턴스 ID와 타임스탬프 기반으로 세션 식별자를 생성하고 효율적인 집계를 위해 데이터를 최적화합니다.
4. **일별 마트 생성 및 다차원 집계 (`final_daily_mart`)**
   - 국가, 플랫폼, 이벤트명 등의 디멘션 조합으로 최종 마트를 구성합니다.

## 📈 분석 픽셀 및 퍼널 정의
본 쿼리는 아래와 같은 이커머스 핵심 전환 퍼널을 트래킹합니다.

- **Bounce**: 앱 오픈 후 상호작용 없는 이탈 세션
- **PDP Visit**: 상품 상세 페이지 방문 세션 (`view_item`, `pdp_detail_view`)
- **Add to Cart**: 장바구니 담기 클릭 세션 (`add_to_cart`)
- **Cart Page Visit**: 장바구니 페이지 진입 세션 (`viewing_cart`)
- **Checkout Page**: 주문 및 결제 시작 세션 (`begin_checkout` 등)
- **Order/Revenue**: 최종 주문 성공 건수 및 매출액 (`purchase`)

## 📁 폴더 구조
```text
app-traffic-analytics/
│
├── .gitignore                  # 로컬 로그 및 CSV 파일 업로드 제외 설정
├── README.md                   # 본 설명서
└── AppTraffic_Overall_Query_v1.6.4.sql  # 마트 생성 핵심 SQL 쿼리
🔒 본 쿼리 내의 고유 로직 및 데이터 구조는 대외비 사항을 포함하고 있으므로 관리에 유의해 주세요.


---

이 `README.md` 파일들을 각 폴더의 최상위에 넣어두고 `git add .` 한 뒤 푸시하시면, GitHub 웹사이트