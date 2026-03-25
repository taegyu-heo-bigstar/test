# 📝 백엔드 기초 개념 및 프로젝트 아키텍처 제안 보고서

## 1. API와 REST API의 이해 (초심자 맞춤 비유)

백엔드 서버의 역할을 이해하기 쉽게 '식당'에 비유해 보겠습니다.

* **API 서버란?**
  [span_0](start_span)API 서버는 간단하게 말하면, 클라이언트의 요청(정확하게는 API 형태에 맞춘 요청)을 처리하여 반환하는 것입니다[span_0](end_span). 식당으로 치면 손님(클라이언트)의 주문을 받고 요리를 내어주는 **'점원'**과 같습니다.
* **REST API란?**
  이 점원과 소통하기 위한 **'표준 주문 규칙'**입니다. 
  * **[span_1](start_span)동작 규칙:** HTTP 메서드로 흔히 알려진 GET, POST, PUT/PATCH, DELETE를 사용해 클라이언트의 요청을 4가지로 분류합니다[span_1](end_span).
  * **[span_2](start_span)예시:** 무언가 정보를 얻고자 할 때는 GET을 사용하며[span_2](end_span)[span_3](start_span), 특정 유저 정보를 얻기 위해 `https://mywebsite/user/{user_id}/` 와 같은 경로를 사용합니다[span_3](end_span). [span_4](start_span)(참고로 GET으로 사용자 정보를 수정하는 것도 가능은 하지만 권장하지 않습니다[span_4](end_span)).

## 2. 개발 프레임워크와 FastAPI

서버를 처음부터 다 짜는 것은 어렵기 때문에 우리는 '웹 프레임워크'라는 도구를 사용합니다. [span_5](start_span)백엔드를 지원하는 프레임워크로는 flask, node.js, next.js, Go, Django, nestjs 등 정말 많은 것이 있습니다[span_5](end_span).

[span_6](start_span)하지만 우리 회의 내용 중에 가장 많이 언급되었던 것은 **FastAPI**입니다[span_6](end_span). [span_7](start_span)우리 프로젝트에 가장 적합한 이유는 다음과 같은 3가지 특징 때문입니다[span_7](end_span):
1. **[span_8](start_span)파이썬 기반 프레임워크:** 파이썬 언어를 기반으로 사용할 수 있어[span_8](end_span)[span_9](start_span), 딥러닝이 돌아갈 환경을 파이썬으로 고려한다면 가장 적합한 선택입니다[span_9](end_span).
2. **[span_10](start_span)RESTful API 완벽 지원:** 프레임워크가 REST API를 온전히 지원합니다[span_10](end_span).
3. **[span_11](start_span)비동기적 처리 지원:** 어떤 요청에 대하여 응답이 올 때까지 기다리는 동기 방식이 아니라, 기다리지 않고 다른 작업을 하는 비동기 방식을 기본적으로 지원합니다[span_11](end_span).

## 3. 필체 교정 앱을 위한 백엔드 구조 제안

[span_12](start_span)일반적으로 클라이언트와 서버 간의 통신이 목적이라면 REST API가 가장 쉽고 문서화가 간단합니다[span_12](end_span). [span_13](start_span)하지만 JSON 기반 통신이기 때문에 이미지 등을 전송하거나 실시간 통신을 하는 데에는 단점이 존재합니다[span_13](end_span). 
[span_14](start_span)우리 프로젝트는 '필체 이미지'를 주고받아야 하므로, REST API 외에도 다양하게 존재하는 통신 방법(MQTT, 웹소켓, gRPC, GraphQL 등) 중에서 gRPC를 활용한 구조를 제안합니다[span_14](end_span).

* **[span_15](start_span)제안하는 서버 아키텍처[span_15](end_span):**
  > **클라이언트 --- REST API --- 일반 서버 --- gRPC --- 딥러닝 서버**

* **[span_16](start_span)구조 설명 및 장점[span_16](end_span):**
  * **일반 서버:** 사용자 계정 정보 등의 저장을 담당합니다.
  * **딥러닝 서버:** 무거운 이미지 연산과 필체 교정 분석을 전담합니다.
  * **gRPC의 활용:** 내부 서버 간 통신에는 gRPC를 사용합니다. gRPC가 바이너리 데이터를 주고받기 때문에 이미지와 같은 데이터를 전송할 때 속도가 빠르다는 큰 장점이 있습니다.
