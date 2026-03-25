# 📝 백엔드 기초 개념 및 프로젝트 아키텍처 제안

## 1. API와 REST API의 이해

백엔드 서버의 역할을 이해하기 쉽게 '식당'에 비유해 보겠습니다.

* **API 서버란?**
  [span_0](start_span)API 서버는 클라이언트의 요청(정확하게는 API 형태에 맞춘 요청)을 처리하여 반환하는 것이라 할 수 있습니다[span_0](end_span). 식당으로 치면 손님의 주문을 받고 요리를 내어주는 **'점원'**과 같습니다.
* **REST API란?**
  이 점원과 소통하기 위한 **'표준 주문 규칙'**입니다.
  * **[span_1](start_span)동작 규칙:** HTTP 메서드로 흔히 알려진 GET, POST, PUT/PATCH, DELETE를 사용해 클라이언트의 요청을 4가지로 분류하고[span_1](end_span)[span_2](start_span), URI(보통은 URL)의 경로에 위치하는 정보를 얻거나 조작합니다[span_2](end_span).
  * **[span_3](start_span)예시:** 무언가 정보를 얻고자 할 때는 GET을 사용합니다[span_3](end_span). [span_4](start_span)예를 들어 특정 유저의 정보를 얻기 위해 `https://mywebsite/user/{user_id}/` 와 같은 경로를 사용합니다[span_4](end_span). [span_5](start_span)(GET이라 쓰고 사용자 정보를 수정하는 것도 가능은 하지만 권장하지는 않습니다[span_5](end_span)).

## 2. 개발 프레임워크와 FastAPI

[span_6](start_span)백엔드를 지원하는 다른 프레임워크로는 flask, node.js, next.js, Go, Django, nestjs 등 정말 많은 것이 있습니다[span_6](end_span). [span_7](start_span)하지만 회의 내용 중에 가장 많이 언급되었던 **FastAPI**가 우리 프로젝트에 가장 적합합니다[span_7](end_span). 그 이유는 다음과 같습니다.

1. **[span_8](start_span)파이썬 기반 프레임워크:** 파이썬 언어를 기반으로 사용할 수 있어[span_8](end_span)[span_9](start_span), 딥러닝이 돌아갈 환경을 파이썬으로 고려한다면 가장 적합한 선택입니다[span_9](end_span).
2. **[span_10](start_span)RESTful API 완벽 지원:** 이 프레임워크가 REST API를 온전히 지원합니다[span_10](end_span).
3. **[span_11](start_span)비동기적 처리 지원:** 어떤 요청에 대하여 응답이 올 때까지 기다리는 동기가 아니라, 비동기를 기본적으로 지원한다고 합니다[span_11](end_span).

## 3. 필체 교정 앱을 위한 백엔드 구조 제안

[span_12](start_span)단순히 클라이언트와 서버 간의 통신이 목적이라면 일반적으로 REST API가 가장 쉽고 문서화가 간단합니다[span_12](end_span). [span_13](start_span)하지만 아무래도 JSON 기반 통신이고, 이미지 등을 전송함에 있어서는 단점도 존재합니다[span_13](end_span). 
[span_14](start_span)우리의 프로젝트 구조를 생각하면 다음의 분리형 서버 구조가 가장 적합하리라 생각합니다[span_14](end_span).

### 🏗️ 제안하는 서버 아키텍처

```mermaid
graph LR
    Client[📱 클라이언트] -- REST API --> AuthServer[💻 일반 서버]
    AuthServer -- gRPC --> DLServer[🧠 딥러닝 서버]
