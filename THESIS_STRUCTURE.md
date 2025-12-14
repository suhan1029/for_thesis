# 논문 작성 가이드: 대규모 언어 모델과 MCP를 활용한 대화형 3D 모델 편집 시스템

## 1. 논문 제목 (Title)
*   **국문:** 대규모 언어 모델과 MCP를 활용한 대화형 3D 모델 편집 시스템 구현
*   **영문:** Implementation of Interactive 3D Model Editing System using Large Language Models and Model Context Protocol

---

## 2. 초록 (Abstract)
본 논문에서는 3D 모델링 소프트웨어의 높은 진입 장벽을 낮추기 위해, 대규모 언어 모델(LLM)과 Model Context Protocol(MCP)을 활용한 대화형 3D 편집 시스템을 제안한다. 사용자가 자연어로 명령을 내리면 LLM이 의도를 파악하여 구조화된 명령어로 변환하고, 이를 로컬 Blender 환경에 전달하여 실시간으로 3D 모델을 수정한다. 제안하는 시스템은 웹 기반 인터페이스와 로컬 3D 엔진을 소켓 통신으로 연결하는 하이브리드 아키텍처를 채택하였으며, 실험을 통해 비전문가도 직관적으로 3D 객체의 속성을 변경할 수 있음을 확인하였다.

---

## 3. 목차 및 주요 내용 (Table of Contents)

### I. 서론 (Introduction)
1.  **연구 배경**
    *   3D 콘텐츠 수요 증가(메타버스, 게임, VR/AR) 대비 전문 툴(Blender, Maya 등)의 높은 학습 난이도.
    *   생성형 AI의 발전으로 텍스트 기반 3D 생성(Text-to-3D)은 가능해졌으나, 생성된 모델을 세밀하게 수정(Editing)하는 것은 여전히 어려움.
2.  **연구 목적**
    *   전문 지식 없이 자연어 대화만으로 3D 모델을 편집할 수 있는 시스템 개발.
    *   LLM의 언어 능력과 기존 3D 툴의 강력한 기능을 결합.
3.  **논문의 구성**

### II. 관련 연구 (Related Work)
1.  **Text-to-3D Generation**
    *   DreamFusion, Magic3D 등 기존 연구의 한계점(수정의 어려움) 언급.
2.  **LLM Agents & Tool Use**
    *   LLM이 외부 도구(API, Calculator 등)를 사용하는 에이전트 연구 동향.
    *   ReAct(Reasoning and Acting) 프롬프팅 기법.
3.  **Model Context Protocol (MCP)**
    *   AI 모델과 애플리케이션/데이터 소스를 연결하는 표준 프로토콜 개념 소개.
    *   본 연구에서는 이 개념을 차용하여 Blender를 하나의 'Tool Context'로 정의.

### III. 시스템 설계 (System Design)
1.  **전체 아키텍처 (Overall Architecture)**
    *   **Frontend (React):** 사용자 채팅 UI 및 Three.js 기반 3D 뷰어.
    *   **Backend (FastAPI):** LLM(Claude) 통신 담당, 프롬프트 관리, MCP Client 역할.
    *   **3D Engine (Blender):** 실제 3D 연산을 수행하는 MCP Server 역할.
2.  **통신 프로토콜 (Communication Protocol)**
    *   Backend ↔ Blender 간의 **Socket 통신** 구조.
    *   **JSON-RPC** 기반 메시지 포맷 정의.
        *   Request: `{ "jsonrpc": "2.0", "method": "change_color", "params": {...}, "id": 1 }`
        *   Response: `{ "jsonrpc": "2.0", "result": "success", "id": 1 }`

### IV. 시스템 구현 (Implementation)
1.  **Blender MCP Server (Addon)**
    *   Python `bpy` 모듈을 활용한 Blender 애드온 개발.
    *   **Socket Server:** 외부 연결을 수신하고 대기하는 서버 구현.
    *   **Command Queue:** Blender의 메인 스레드(Main Thread) 안전성을 보장하기 위한 큐 기반 명령 처리 패턴.
    *   **기능 구현:** `change_color`, `scale_model`, `rotate_model`, `apply_smooth` 등 주요 편집 기능의 API화.
2.  **LLM 기반 의도 파악 (MCP Client)**
    *   **System Prompt Engineering:** LLM에게 'Blender 전문가' 페르소나 부여.
    *   **Tool Definition:** 사용 가능한 함수 목록을 JSON Schema 형태로 LLM에게 제공.
    *   **In-Context Learning:** 자연어 요청을 정확한 JSON 파라미터로 변환하기 위한 Few-shot 예시 제공.

### V. 실험 및 결과 (Experiments & Results)
1.  **실험 환경**
    *   OS: Windows, GPU 환경, Blender 4.x, Python 3.10+.
2.  **기능 테스트 (Case Studies)**
    *   **Case 1: 속성 변경** ("이 모델 빨간색으로 바꿔줘" → 색상 변경 성공 여부).
    *   **Case 2: 기하학적 변환** ("크기 두 배로 키우고 45도 돌려줘" → Scale/Rotate 복합 명령 처리).
    *   **Case 3: 객체 조작** ("표면 부드럽게 해줘" → Modifier 적용 여부).
3.  **결과 분석**
    *   명령어 인식 성공률 및 실행 속도.
    *   실제 Blender 뷰포트 화면과 웹 뷰어 화면의 동기화 결과.

### VI. 결론 (Conclusion)
1.  **연구 요약**
    *   LLM과 MCP를 활용하여 직관적인 3D 편집 인터페이스를 성공적으로 구현함.
2.  **기대 효과**
    *   3D 콘텐츠 제작의 대중화 기여.
3.  **향후 과제**
    *   더 복잡한 모델링 기능(Vertex/Face 단위 편집) 지원.
    *   다중 사용자 지원 및 클라우드 렌더링 연동.

---

## 4. 논문에 포함할 그림 (Figures) 제안

1.  **Figure 1. 시스템 전체 구성도:**
    *   [User] ↔ [React Frontend] ↔ [FastAPI Backend (LLM)] ↔ [Socket] ↔ [Blender (MCP Server)] 흐름도.
2.  **Figure 2. 프롬프트 구조 예시:**
    *   System Prompt에 정의된 도구(Tool) 명세와 LLM이 생성한 JSON 응답 예시.
3.  **Figure 3. Blender 애드온 내부 구조:**
    *   Socket Listener → Command Queue → Main Thread Execution 흐름.
4.  **Figure 4. 실행 결과 화면:**
    *   (좌) 채팅 입력 창 / (우) 변경된 3D 모델 결과 (Before & After).
