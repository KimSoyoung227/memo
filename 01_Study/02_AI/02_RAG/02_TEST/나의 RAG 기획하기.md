#로컬LLM기획 #Ollama #QwenCoder #AnythingLLM #ContinueDev

# 나의 요구 사항
###### 개인용 SLM 용도 사용하므로 개인정보 보안에 강할 것
1. Ollama
	터미널 기반이므로 가볍고, 
		-로컬 실행 중심이다
		-API처럼 Spring/Python에서 붙이기 쉽다
	- LM Studio
		-GUI가 편하다
		-모델 검색/다운로드/채팅이 쉽다
		-앱은 오프라인 실행이 가능하다
	- GPT4II
		-일반 사용자에게 편하다
		-데스크톱에서 프라이빗으로 실행, API 호출 없이 사용 가능하다
2. 코딩 작업을 요청하고 GitHub 연동이 가능할 것 
	- Ollama + Continue.dev + Aider(로컬 모델 + 코딩 에이전트 + GitHub 연동)
		-Ollama : 로컬 LLM 실행하는 모델 실행기
		-Continue.dev : VS Code/JetBrains 지원하는 플러그인, 터미널에서 코딩 보조, 깃헙 PR/검토 자동화
		-Aider : Git repo 기반으로 코드 수정하는 터미널형 AI 페어 프로그래머. 실제 파일 수정/커밋 단위 작업 수행
3. 일상용으로도 사용하며 내 기록을 학습시켜 나만의 모델을 만들 것 (추후 파인튜닝 가능)
	- 개인 지식 저장소(RAG)
```
Ollama
 |- 코딩용 : Continue.dev / Aider
 |_ 일상용 : AnythingLLM 또는 Open WebUI 
 
 //AnythingLLM : ChatGPT처럼 인터페이스 + 문서검색 + 에이전트 기능을 제공하는 플랫폼. 모델, 문서, 채팅을 로컬 데스크톱에 저장하는 구조. Ollama모델과 연결 가능
```


___
# RAG 구축을 위해 필요한 재료

## 재료는 무엇이 필요할까?
1. 모델/엔진(Qwen) : 질문 -> 모델/엔진 -> 답변
2. 모델/엔진을 실행해주는 프로그램(Ollama) : 모델/엔진 관리기
	- 모델/엔진 다운로드
	- 모델/엔진 목록 관리
	- 모델/엔진 실행
	- 로컬 API 서버 제공
	- 메모리 관리
3. AI 인터페이스(AnythingLLM, Continue.dev) : 모델과 사용자간 연결 인터페이스
	- 화면 제공 프로그램(AnythingLLM) : ChatGPT 같은 화면을 제공
		-채팅창
		-문서 업로드
		-문서 분석
		-검색 인덱스 생성
		-대화 기록 저장
		-질문 시 문서 검색
		-RAG
		-워크스페이스
	- IDE에서 지원하는 플러그인(Continue.dev) : IDE에서 연동하여 추가 작업을 최소화해주는 플러그인. 깃 PR/Merge를 자동화하거나 코딩 리뷰 등 개발 작업에 도움을 주는 비서같은 기능 모음집이다.
		-터미널에서 작동
		-질문시 코딩 보조

>Continue.dev는 아래와 같이 코드 상담과 같은 기능에 적합하다.
 >- 코드 설명
 >- 코드 생성
 >- 에러 설명
 >- 학습
   
 >Aider는 아래와 같이 코드 작업과 같은 기능에 적합하다.
 >- 기능 구현
 >- 리팩토링
 >- 버그 수정
 >- 테스트 코드 생성
 
```
목적에 따라 재료가 달라진다
보통 모델/엔진은 코딩에 최적화된 모델을 별도로 지원한다. 문서 검색(RAG)는 검색용 모델이 적합하며, 추론 능력이 좋은 모델이 별도로 있을 것이다.
```
[코드용]
VS Code  
↓  
Continue.dev  
↓  
Ollama  
↓  
Qwen Coder  
  
-----------------  
[RAG용] 
AnythingLLM  
↓  
Ollama  
↓  
Qwen

_________________
[에이전트용]
Claud Code


```
Ollama를 모델 관리기로 사용하되 사용 목적에 따라 모델을 실행해준다.
코딩할 때에는 AnythingLLM같은 GUI프로그램은 필요 없으므로 플러그인(Continue.dev)을 사용하고 문서 검색에서는 가독성과 대화 관리해주는 GUI 프로그램(AnythingLLM)을 사용하는 것이 좋다.

>**Q. Ollama만 있으면?**
>A. 모델 실행 가능, 문서 관리 없음, 채팅 기록 없으므로 터미널에서 가독성 없이 실행해야 한다.

>**Q. AnythingLLM만 있으면?**
>A. 실행 가능한 모델이 없으므로 문답이 불가능하다.



___
# 모델 추천 받기
1. "모델을 사용한다" 란?
	-  로컬 모델 사용(API 토큰 불필요)
		-AnythingLLM + Ollama + Qwen 2.5 7B
			-모델 파일이 내 PC에 있음
			-GPU/CPU로 직접 실행
			-인터넷 연결 불필요
			-API 비용 없음
			-API 토큰 없음
	-  클라우드 모델 사용(API 토큰 필요)
		-AnythingLLM + OpenAI GPT5.5/Anthropic Cluade API
			-모델은 서버에 있음
			-API key 필요
			-사용량만큼 과금
			-데이터가 외부 서버로 전송됨
			-추론력과 복잡한 문제 해결 우세
2. 나에게 맞는 모델은?
	- Qwen Coder : 중국의 Alibaba가 만든 LLM. 일상 대화와 코딩 보조에 적합
	- Llama : 미국의 Meta에서 만든 LLM. 생태계가 가장 크다. 영어 위주이며 일반 채팅/RAG/연구용에 적합
	- DeepSeek : 중국의 DeepSeek가 만든 LLM 코딩 집중에 적합

___
# 추후 활용 가능해 지는 범위

- AI 에이전트 서비스 개발
```
[프로세스]
사용자 질문 -> 크롤링 -> DB 저장 -> 분석 -> 응답

__________________________________________

[실제 기술]
클라우드 AI API 또는 Qwen과 같은 무료 모델 + Langraph + MCP

// LanGraph : AI가 의사 결정하는 워크플로우를 만드는 프레임워크
// MCP(Model Context Protocol) : AI와 외부 도구(AWS, GitHub, filesystem)와 대화하는 표준 규격. Anthropic이 개발한 무료오픈소스이나 MCP로 연결하는 대상 서비스는 유료(AWS)일 수 있음.