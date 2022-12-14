# HTTP

- HyperText Transfer Protocol
- Image,음성,영상 ,파일 ,JSON ,XML , 거의 모든 형태의 데이터 전송가능
- 서버간에 데이터를 주고받을떄도 대부분 HTTP 사용
- HTTP 1.1이 가장 많이 사용

# HTTP 프로토콜 버전

- HTTP 1.1 / HTTP 2 => TCP 기반 (3 handshake를 해야되서 HTTP3에비해 무거움)
- HTTP/3 => UDP 기반

# HTTP 특징

- 클라이언트 서버 구조
- 스테이리스 프로토콜 , 비연결성 모델
- HTTP메시지 , 단순함 확장가능

## Stateless 프로토콜

- 서버가 클라이언트 상태를 보존하지 x
- 클라이언트의 어떤 요청에도 서버는 알아들을수있음
- 상태유지를 신경안써도 되서 서버 증설이 쉬움
  - why? : 상태 유지를 안하면 서버가 장애가 나도 어떤 서버에도 요청을할수있음 , 상태유지를 하면 하나의 서버와 클라이언트간에 연결이 되있어야함

> Stateful 같은경우는 로그인 했다는 상태를 서버가 유지해야되는경우 (쿠키와 서버 세션을 이용하여 상태유지한다.)

## 비연결성 모델의 장점

- 클라이언트와 서버간의 요청 응답후 종료됨으로 연결유지를 하지않아 서버의 리소스가 절약됨

> But Tcp/IP연결을 새로 계속 맺어야하는 단점이 발생하여 HTTP에서는 지속연결도입으로 문제로 해결하게됬다

## HTTP 지속 연결

- 클라이언트에서 서버에게 여러요청을 전달하고 서버는 응답후 종료가된다. (파이프라이닝)
- HTTP 1.1에서는 모든연결이 지속연결로 간주된다.

# HTTP 메시지 (중요)

1. start-line (시작라인)
2. header(헤더)
3. 공백라인 (CRLF)
4.messagebody으로 구성

## HTTP 메시지 (start-line : 시작라인)

- request-line / status-line으로 구별
  - request-line : http method 종류 , http version, requerst-target(요청대상) ,CRLF(엔터)
  - status-line : http 버전 , http 상태코드

## HTTP 메시지 헤더 (header)

- HTTP 전송에 필요한 모든 부가정보가 들어가있음
  - 메시지바디크기 ,압축 ,인증 정보 ,클라이언트 정보 ,서버애플리케이션 정보,캐시정보등...
  > 메시지 바디에 대한 내용뺴고 모든 메타정보가 들어가있다

## HTTP 메시지 (message-body)

- 실제 전송할 데이터 (HTML,문서,이미지 ,영상,Json  ) =>byte로 표현할수있는 모든데이터
