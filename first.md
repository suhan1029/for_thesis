# 논문

## 대화형 3D 모델 편집 시스템 구현 (Implementation of Interactive 3D Model Editing System)

본 연구의 목적은 3D 모델링 소프트웨어의 높은 진입 장벽을 낮추기 위해, 대규모 언어 모델(LLM)과 Model Context Protocol(MCP)을 활용한 대화형 3D 편집 시스템을 제안하는 것이다. 사용자가 자연어로 명령을 내리면 LLM이 의도를 파악하여 구조화된 명령어로 변환하고, 이를 로컬 Blender 환경에 전달하여 실시간으로 3D 모델을 수정한다. 제안하는 시스템은 웹 기반 인터페이스와 로컬 3D 엔진을 소켓 통신으로 연결하는 하이브리드 아키텍처를 채택하며, 실험을 통해 비전문가도 직관적으로 3D 객체의 속성을 변경할 수 있음을 확인한다.

The purpose of this study is to lower the high entry barrier of 3D modeling software by proposing an interactive 3D editing system that leverages Large Language Models (LLMs) and the Model Context Protocol (MCP). When users issue commands in natural language, the LLM interprets the user’s intent and translates it into structured commands, which are then transmitted to a local Blender environment to modify 3D models in real time. The proposed system adopts a hybrid architecture that connects a web-based interface with a local 3D engine via socket communication. Experimental results demonstrate that even non-expert users can intuitively modify the properties of 3D objects using the proposed approach.

---

### 키워드

3D 모델링, LLM, MCP, Blender, 하이브리드 아키텍처

---

### I. 서론

1. 연구 배경
 - 사람들이 온라인에서 콘텐츠를 소비하는 시간이 많아 지면서 3D 콘텐츠에 대한 수요도 증가하고 있다.(사람들의 온라인 콘텐츠 소비 시간 증가가 있는 논문, 3D 콘텐츠 수요 증가가 있는 논문)
 - 하지만 이러한 3D 콘텐츠를 만드는 툴의 경우 학습 난이도가 높고(예: Blender), 콘텐츠 제작 시간도 오래 걸린다.(Blender에 대한 논문, 학습 난이도, 3D 콘텐츠 제작 시간에 대한 논문)
 - 생성형 AI의 발전으로 텍스트 기반 3D 생성(Text-to-3D), 이미지 기반 3D 생성(Image-to-3D)이 가능해졌으나, 생성된 모델을 세밀하게 수정하는 것은 여전히 어렵다.(3D 모델에 대한 논문)

2. 연구 목적
 - 그래서 전문 지식 없이 이미지를 올려서 3D 모델을 생성하고, 자연어 대화만으로 생성된 3D 모델을 편집할 수 있는 통합 시스템을 개발하는 것이 본 연구의 목적이다. 
 - LLM의 언어 능력과 기존 3D 툴의 강력한 기능을 결합하는 것으로, 이 연구는 다음과 같은 2가지 목표를 달성할 수 있다.
 - 첫째, 일반인이 별도의 지식 학습 없이 자신이 원하는 3D 모델을 가질 수 있다.
 - 둘째, 3D 모델링 전문가들도 모델을 처음부터 직접 제작하는 것이 아닌 생성된 3D 모델을 자연어로 편집함으로써 같은 시간에 더 많은 3D 모델을 만들 수 있다.

---

### II. 선행 연구

본 연구와 밀접하게 관련된 아래의 연구들을 소개한다.

1. unik-3D

2. LLM

3. MCP
3-1. MCP 사례 1
3-2. MCP 사례 2

---

### III. 시스템 설계 

본 연구의 아키텍처는 아래와 같다.

(전체 시스템 아키텍처 그림)

(Frontend, Backend, AI, Blender 부분으로 나누어서 각각 어떤 도구를 사용하고 서로 어떻게 연결되는지 설명하기)

백엔드와 Blender는 소켓 통신 구조로 연결되는데, 그 통신 프로토콜을 보면 다음과 같다.(통신 프로토콜을 설명하는 논문)

(소켓 통신 구조에 대한 설명)

(백엔드와 Blender의 통신 구조에 대한 자세한 설명)

(JSON-RPC** 기반 메시지 포맷 정의)

---

### IV. 시스템 구현 























