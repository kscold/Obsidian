- gRPC = Google이 만든 **고성능 RPC 프레임워크**. Protocol Buffers(.proto)로 인터페이스를 정의하고 HTTP/2 위에서 바이너리로 통신한다.
- [[AI Agent|에이전트]] 서버에서 자주 등장 — Spring/Java 백엔드 ↔ Python LLM 서버 같이 **언어가 다른 두 서비스를 강한 타입 계약으로 연결**할 때 REST보다 유리.

## 왜 REST 대신 gRPC인가

| | REST/JSON | gRPC/Protobuf |
|--|-----------|----------------|
| 전송 | HTTP/1.1 + JSON | HTTP/2 + binary |
| 스키마 | OpenAPI(선택) | .proto(필수) |
| 스트리밍 | SSE/WebSocket 부가 | 1급 지원 (4가지 모드) |
| 페이로드 | 큼 | 작음(2~10x) |
| 코드 생성 | 선택 | 필수 (강타입 클라이언트) |
| 브라우저 | 직접 | gRPC-Web 필요 |

## .proto 정의

```protobuf
syntax = "proto3";
package agent;

service AgentService {
  rpc Chat(ChatRequest) returns (ChatResponse);
  rpc StreamChat(ChatRequest) returns (stream ChatChunk);   // 서버 스트림
}

message ChatRequest {
  string user_id = 1;
  string message = 2;
}
message ChatResponse {
  string answer = 1;
  repeated string citations = 2;
}
message ChatChunk {
  string delta = 1;
}
```

## Python 서버

```python
import grpc
from concurrent import futures
import agent_pb2, agent_pb2_grpc

class AgentService(agent_pb2_grpc.AgentServiceServicer):
    def Chat(self, request, context):
        ans = agent.invoke(request.message)
        return agent_pb2.ChatResponse(answer=ans)

    def StreamChat(self, request, context):
        for tok in stream_answer(request.message):
            yield agent_pb2.ChatChunk(delta=tok)

server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
agent_pb2_grpc.add_AgentServiceServicer_to_server(AgentService(), server)
server.add_insecure_port("[::]:50051")
server.start()
server.wait_for_termination()
```

## Python 비동기 서버 (grpcio.aio)

```python
async def serve():
    server = grpc.aio.server()
    agent_pb2_grpc.add_AgentServiceServicer_to_server(AgentService(), server)
    server.add_insecure_port("[::]:50051")
    await server.start()
    await server.wait_for_termination()
```

- 비동기 서버는 [[async-await|asyncio]] 친화 → LLM I/O 대기와 자연스럽게 결합.

## 스트리밍 4모드

| 모드 | 입력 | 출력 |
|------|------|------|
| Unary | 단건 | 단건 |
| Server streaming | 단건 | stream | ← LLM 응답에 가장 자주 |
| Client streaming | stream | 단건 |
| Bidirectional | stream | stream | ← 양방향 대화에 적합 |

## 성능·운영 팁

- **Keepalive** + HTTP/2 multiplexing → 같은 연결로 여러 요청 다중화.
- **Deadline** — 모든 호출에 timeout 설정. LLM은 응답 시간이 변동성 크니 필수.
- **Reflection** — 운영에서 `grpcurl`로 디버깅하려면 reflection 서비스 등록.
- **Interceptor** — 인증·로깅·메트릭을 미들웨어처럼.

## 흔한 함정

- proto 변경 시 양쪽 코드 재생성 잊음 → 런타임 에러.
- 큰 페이로드(이미지 등)를 message 안에 그대로 → byte 한도(default 4MB) 초과. 외부 스토리지 + URL 권장.
- 브라우저 직접 호출 불가 → gRPC-Web 또는 별도 REST gateway 필요.

## 관련

- [[async-await]] — gRPC aio 서버.
- [[Streaming]] — LLM 토큰 스트리밍을 gRPC server streaming으로 전달.
- [[Observability]] — gRPC interceptor로 트레이스 삽입.
