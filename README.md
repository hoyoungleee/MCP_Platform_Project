# 🤖 MCP(Model Context Protocol) 기반 AI 에이전트 플랫폼

**Java/Spring으로 구현하는, 외부 도구와 연동하여 실제 작업을 수행하는 AI 에이전트 플랫폼 구축 프로젝트**

## 🌟 프로젝트 개요 (Overview)

본 프로젝트는 단순히 챗봇처럼 대답만 하는 AI를 넘어, 외부 데이터 및 서비스(API, DB 등)와 능동적으로 상호작용하여 복잡한 작업을 수행할 수 있는 **AI 에이전트 플랫폼**을 구축하는 것을 목표로 합니다.

이러한 상호작용의 표준으로 **MCP (Model Context Protocol)** 개념을 적용하여, AI 모델이 외부 도구를 일관되고 확장 가능한 방식으로 사용할 수 있는 아키텍처를 설계하고 구현합니다.

## 💡 핵심 개념 (Core Concepts)

### MCP (Model Context Protocol)
MCP는 AI 모델이 외부 시스템과 상호작용하는 과정을 표준화한 **'통신 규약'**입니다. 이를 통해 어떤 도구나 데이터 소스든 AI 에이전트에 쉽게 연결하여 활용할 수 있습니다.

### 주요 구성 요소 및 흐름
- **Host**: AI 모델(LLM)을 실행하고 전체 작업을 관리하는 주체입니다.
- **Client**: Host의 요청을 외부 시스템(Server)에 전달하고 결과를 반환하는 중개자입니다.
- **Server**: Client의 요청을 실제로 처리하는 외부 시스템(API, DB 등)입니다.

**📡 워크플로우:**
`AI 모델 (Host)` → `클라이언트` → `서버` → `클라이언트` → `AI 모델 (Host)`

이 흐름을 통해 AI는 더 풍부한 맥락(Context)을 가지고 정확한 추론과 작업 수행이 가능해집니다.

## ✨ 주요 기능 (Features)

- **🔧 도구(Tool / Server) 관리**
  - REST API, DB 등 외부 도구를 플랫폼에 등록 및 관리
  - 등록된 도구의 유효성을 검증하는 테스트 기능

- **🤖 AI 에이전트(Host) 관리**
  - 사용할 LLM 모델(OpenAI, Anthropic 등) 선택 및 에이전트 생성
  - 생성된 에이전트가 사용할 수 있는 도구를 선택적으로 연결

- **🚀 워크플로우 실행 및 관리**
  - 채팅 UI를 통해 사용자가 자연어로 작업을 요청
  - 요청을 해석하여 적절한 도구를 동적으로 호출하는 실행 엔진 구현
  - 작업 완료 후 사용자에게 최종 결과 반환

- **📊 모니터링 및 로그**
  - 에이전트의 작업 흐름(생각, 도구 호출, 결과)을 추적하는 로그 시스템
  - 디버깅 및 분석을 위한 실행 과정 시각화

## 🏛️ 시스템 아키텍처 (System Architecture)

본 플랫폼은 아래와 같은 아키텍처로 구성됩니다.

```mermaid
graph TD
    subgraph Browser
        A[👤 User]
    end

    subgraph Frontend
        B[React / Next.js]
    end
    
    subgraph Backend [Backend Server]
        C{Spring Boot API Gateway}
        D[🤖 Agent Executor (LangChain4j)]
        E[(DB: PostgreSQL)]
    end

    subgraph External Services
        F[🛠️ External Tools (APIs, DBs)]
        G[🧠 LLM API (OpenAI, etc.)]
    end
    
    A -- HTTP Request --> B
    B -- REST API Call --> C
    C -- Forward Request --> D
    D -- 1. Get Tool Info --> E
    D -- 2. Execute Tool --> F
    F -- 3. Return Result --> D
    D -- 4. Think with LLM --> G
    G -- 5. Return Thought --> D
    D -- 6. Generate Final Answer --> C
    C -- Log Interaction --> E
    C -- API Response --> B
```

## 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 | 선정 이유 |
| :--- | :--- | :--- |
| **Frontend** | `React` / `Next.js` | 컴포넌트 기반의 UI 개발, 풍부한 생태계 |
| **Backend** | `Java 17+`, `Spring Boot 3.x` | 안정적이고 견고한 엔터프라이즈급 백엔드 구축 |
| **AI/LLM** | `LangChain4j` | Java 환경에서 LLM과 외부 도구를 쉽게 통합 |
| **Database** | `PostgreSQL` | 신뢰성 높은 관계형 데이터 저장 및 관리 |
| **Deployment**| `Docker`, `AWS/GCP` | 컨테이너 기반의 일관성 있는 배포 및 확장성 확보 |

## 🚀 앞으로의 개발 계획 (Roadmap)

- [ ] **Phase 1: MVP 개발**
  - [ ] 단일 Tool 등록 및 테스트 기능 구현
  - [ ] 단일 Agent 생성 및 Tool 연결 기능 구현
  - [ ] 간단한 1-Step 작업 실행 및 로그 확인 기능

- [ ] **Phase 2: 기능 고도화**
  - [ ] 다중 Tool을 순차적으로 사용하는 Multi-Step 워크플로우 지원
  - [ ] Vector DB를 연동한 RAG(Retrieval-Augmented Generation) 기능 추가
  - [ ] 사용자 인증/인가 시스템 도입

- [ ] **Phase 3: 사용성 개선**
  - [ ] GUI 기반의 워크플로우 빌더 도입
  - [ ] 대시보드를 통한 에이전트 사용 통계 및 모니터링 기능 강화