# 🚀 Crypto Function Calling Agent

실시간 가상자산 시장 정보, 뉴스 검색, 용어 설명, 사용자 전략 조회를 지원하는 **멀티턴 기반 Function Calling Agent 챗봇** 프로젝트입니다.

LLM이 직접 답변을 생성하는 방식이 아니라, 사용자의 질문에 맞는 Tool을 선택하고 실행 결과를 기반으로 응답하도록 설계했습니다.

---

## 📌 프로젝트 배경

코인 시세, 뉴스, 전략 등 가상자산 관련 정보는 다양한 데이터 소스에 분산되어 있습니다. 일반 LLM은 실시간 데이터 접근이 어렵기 때문에 Function Calling 없이 답변할 경우 정확도가 낮아지고 hallucination이 발생할 수 있습니다.

또한 기존 Prompt 기반 Function Calling은 Tool 호출을 안정적으로 제어하기 어려워, 이를 해결하기 위해 **Function Calling 데이터로 파인튜닝한 Agent 시스템**을 구축했습니다.

---

## 🎯 프로젝트 목표

LLM이 직접 답변하는 것이 아니라 상황에 맞는 Tool을 정확하게 호출하도록 학습된 Agent를 구축하고, API·RAG·DB를 통합한 멀티턴 기반 실시간 금융 정보 서비스를 구현하는 것을 목표로 했습니다.

---

## 🚀 주요 기능

- 실시간 코인 시세 조회
- 과거 가격 조회
- 24시간 통계 조회
- 급등락 코인 조회
- 트렌드 코인 조회
- 시장 스냅샷 조회
- 코인 비교
- 암호화폐 뉴스 검색 및 요약
- 암호화폐/경제 용어 검색
- 사용자 프로필 및 최근 전략 조회
- 멀티턴 대화 기반 Tool 호출

---

## 👨‍💻 담당 역할 및 기여사항

- 멀티턴 Function Calling 데이터셋 구축
- OpenAI Function Calling 포맷 기반 학습 데이터 전처리
- Qwen2.5-7B-Instruct 모델 LoRA 파인튜닝
- BFCL 기반 모델 성능 평가
- FastAPI 기반 Agent API 서버 구현
- Tool Dispatcher 및 Argument Normalization 로직 구현
- FAISS 기반 뉴스/용어 RAG 시스템 구축
- JWT 기반 사용자 인증 및 DB 연동
- vLLM 기반 추론 서버 연동

---

## 🧠 시스템 구조

```text
User Query
   ↓
FastAPI API Server
   ↓
Agent Runner
   ↓
LLM Tool Selection
   ↓
Argument Normalization
   ↓
Tool Dispatcher
   ↓
 ┌─────────────────┬─────────────────┬─────────────────┐
 │ Market API      │ RAG Search      │ User DB         │
 │ Price / Market  │ News / Terms    │ Profile/Strategy│
 └─────────────────┴─────────────────┴─────────────────┘
   ↓
Tool Result
   ↓
LLM Response Generation
   ↓
Final Answer
```
---
# 📁 프로젝트 전체 구조
```bash
investMario_chatbot/
├── backend/                                    # 백엔드 메인 서버
│   ├── app/
│   │   └── agent/                             # AI 에이전트 시스템
│   │       ├── agent_runner.py                # 에이전트 실행 엔진
│   │       ├── llm.py                         # LLM 통합 모듈
│   │       ├── prompt.py                      # 프롬프트 템플릿
│   │       ├── schemas.py                     # 데이터 스키마 정의
│   │       ├── date_utils.py                  # 날짜 유틸리티
│   │       ├── tool_registry.py               # 도구 레지스트리
│   │       ├── tool_dispatcher.py             # 도구 디스패처
│   │       ├── tool_logic.py                  # 도구 로직 구현
│   │       ├── tools_market.py                # 시장 정보 도구
│   │       ├── tools_news.py                  # 뉴스 관련 도구
│   │       ├── tools_portfolio.py             # 포트폴리오 도구
│   │       └── tools_terms.py                 # 암호화폐 용어 도구
│   ├── crypto_news_db/                        # 암호화폐 뉴스 데이터베이스
│   │   ├── index.faiss                        # FAISS 인덱스
│   │   └── index.pkl                          # 메타데이터 저장소
│   ├── crypto_term_db/                        # 암호화폐 용어 데이터베이스
│   │   ├── index.faiss                        # FAISS 인덱스
│   │   └── index.pkl                          # 메타데이터 저장소
│   ├── main.py                                # 메인 애플리케이션 진입점
│   └── requirements.txt                       # Python 의존성
├── function_calling/                          # 함수 호출 파인튜닝 관련
│   ├── crypto_base_model_eval.ipynb           # 기본 모델 평가
│   ├── crypto_finetuned_model_eval.ipynb      # 파인튠 모델 평가
│   ├── crypto_fuction_calling_finetuning.ipynb # 함수 호출 파인튜닝
│   ├── 가상자산_펑션콜링_데이터셋.ipynb      # 가상자산 펑션콜링 데이터셋
│   ├── merge_upload.ipynb                     # 모델 병합 및 업로드
│   └── evaluation_results.csv                 # 평가 결과
├── qwen-2.5-7b-function-calling/              # Qwen 파인튜닝 모델
│   └── merged/                                # 병합된 모델
│       ├── config.json                        # 모델 설정
│       ├── generation_config.json             # 생성 설정
│       ├── added_tokens.json                  # 추가 토큰
│       ├── special_tokens_map.json            # 특수 토큰 맵
│       ├── model.safetensors.index.json       # 모델 인덱스
│       ├── tokenizer.json                     # 토크나이저
│       ├── tokenizer_config.json              # 토크나이저 설정
│       ├── vocab.json                         # 어휘 집합
│       └── merges.txt                         # BPE 머지 정보
├── .git/
├── .gitignore
└── README.md
```

# 🧩 Function Calling 모델 파인튜닝

멀티턴 대화 기반 Function Calling 데이터를 직접 구축하고 OpenAI 포맷으로 변환하여 Qwen2.5-7B-Instruct 모델을 LoRA 방식으로 파인튜닝했습니다.

이를 통해 모델이 다음 능력을 학습하도록 구성했습니다.

- Tool 선택
- Argument 생성
- 자연어 응답 생성
- Tool Call 생성
- 멀티턴 문맥 유지
- 안전한 거절 응답

---

# 📊 모델 평가

## ⎖ BFCL Metric - 기본 모델

| Metric | Score |
|---|---|
| Total Samples | 224 |
| Tool Accuracy | 93.30% |
| Param Key Accuracy | 83.28% |
| Param Value Accuracy | 52.01% |
| Hallucination Rate | 7.59% |
| Over-call Rate | 0.85% |
| Under-call Rate | 3.08% |
| Format Error Rate | 0.00% |

---

## ⎖ BFCL Metric - 파인튜닝 모델

| Metric | Score |
|---|---|
| Total Samples | 242 |
| Tool Accuracy | 100% |
| Param Key Accuracy | 100% |
| Param Value Accuracy | 94.21% |
| Hallucination Rate | 0.00% |
| Over-call Rate | 0.34% |
| Under-call Rate | 0.68% |
| Format Error Rate | 0.00% |

---

## ⎖ 시나리오 평가

| Metric | Score |
|---|---|
| 시나리오 샘플 수 | 52 |
| 평균 도구 정확도 | 0.861 |
| 응답 유효성 | 1.000 |
| 종합 점수 | 0.903 |

---

# 🛠️ 기술 스택

| Category | Stack |
|---|---|
| LLM | Qwen2.5-7B-Instruct |
| Fine-tuning | LoRA, SFTTrainer, ChatML |
| Backend | FastAPI, Python |
| Inference | vLLM, RunPod |
| RAG | FAISS, LangChain |
| Database | PostgreSQL, SQLAlchemy |
| API | CoinGecko API, REST API |
| Version Control | Git, GitHub |

# 팀 프로젝트

[https://github.com/HJJunn/investMario_chatbot](https://github.com/HJJunn/investMario)

---


