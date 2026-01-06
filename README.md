## Technical Architecture 🏗️

Deepduck은 **Graph-based Autonomous Payload-Driven Vulnerability Scanner**입니다.  
기존의 단순한 fuzzing 도구와 달리, **지식 그래프(Knowledge Graph)**와 **LLM(Large Language Model)**을 활용하여 사이트 구조를 학습하고, 상황에 맞는 **최적의 페이로드 공격 시나리오**를 자동으로 생성·실행합니다.

핵심 철학은 **"The Flock" (오리 떼 군집 지능)** — 단일 프로세스가 아닌, 역할이 분담된 여러 Worker들이 협업하여 타겟을 정복합니다.

### 1. System Overview (시스템 개요)
- **Input**: 단일 타겟 URL (예: `http://example.com`)
- **Output**: Markdown 보고서 (발견된 취약점, 사용된 페이로드, 성공률, 전체 커버리지)
- **Dual Mode**:
  - **Safe Mode** — 실제 사이트 소유자용 (destructive payload 차단, rate limiting, 소유권 검증)
  - **Full Mode** — 모의 해킹 랩(HTB, PortSwigger, pentest-ground 등)용 (무제한 페이로드)

### 2. Core Architecture: The Flock (코어 아키텍처)

TaskQueue(Priority Queue)를 중심으로 3가지 Worker가 비동기적으로 상호작용합니다.

#### 🧬 Central Nervous System (중추 신경계)
1. **Task Queue (우선순위 큐)**  
   - 모든 작업(탐색, 분석, 공격)을 관리하는 중앙 통제소  
   - PriorityQueue 사용: 로그인 폼 분석(P=3) > 단순 링크 수집(P=5)

2. **Knowledge Graph (지식 그래프)**  
   - NetworkX 기반 인메모리 그래프 DB  
   - Nodes: URL, Form, Parameter, DOM 요소  
   - Edges: link_click, form_submit, redirect, parameter_injection  
   - 탐색이 진행될수록 사이트의 '지도'가 완성되며, 중복 탐색 방지

#### 🕵️ Explorer Worker (탐험가)
- **Role**: 사이트 구조 파악 및 노드 확장
- **Tech**: Selenium (Headless Chrome) + BeautifulSoup + DOM Hashing
- **Logic**:
  - URL 방문 → DOM 파싱 → 새로운 링크, 폼, 파라미터 추출
  - Anti-Rabbit Hole: compute_dom_hash()로 구조가 동일한 페이지(페이징, 캘린더 등) 무한 루프 방지
  - scope.yaml 정책 준수 (
