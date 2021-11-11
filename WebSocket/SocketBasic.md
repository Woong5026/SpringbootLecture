### WebSocket 접속 과정

![image](https://user-images.githubusercontent.com/78454649/141267135-c1711a7c-f89c-45df-9f3a-38153322bd8b.png)

웹소켓을 이용하여 서버와 클라이언트가 통신을 하려면 먼저 웹소켓 접속 과정을 거쳐야 한다. 

웹소켓 접속 과정은 TCP/IP 접속 그리고 웹소켓 열기 HandShake 과정으로 나눌 수 있다

 TCP/IP 접속이 완료된 후 서버와 클라이언트는 웹소켓 열기 HandShake 과정을 시작한다.
 
 - 웹소켓 열기 HandShake

웹소켓 열기 핸드셰이크는 클라이언트가 먼저 핸드셰이크 요청을 보내고 이에 대한 응답을 서버가 클라이언트로 보내는 구조이다.

### WebSocket 구현 테스트

1. 라이브러리 추가

2. WebSocket Handler

소켓통신은 서버와 클라이언트가 1:N의 관계를 맺는다. 즉, 하나의 서버에 다수 클라이언트가 접속할 수 있다. 

따라서 서버는 다수의 클라이언트가 보낸 메세지를 처리할 핸들러가 필요하다. 

텍스트 기반의 채팅을 구현해볼 것 이므로 'TextWebSocketHandler'를 상속받아서 작성한다. 

Client로부터 받은 메세지를 Log출력하고 클라이언트에게 환영하는 메세지를 보내는 역할을 한다.

```java
@Component
@Log4j2
public class ChatHandler extends TextWebSocketHandler {

    private static List<WebSocketSession> list = new ArrayList<>();

    @Override
    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        String payload = message.getPayload();
        log.info("payload : " + payload);

        for(WebSocketSession sess: list) {
            sess.sendMessage(message);
        }
    }

    /* Client가 접속 시 호출되는 메서드 */
    @Override
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {

        list.add(session);

        log.info(session + " 클라이언트 접속");
    }

    /* Client가 접속 해제 시 호출되는 메서드드 */

    @Override
    public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {

        log.info(session + " 클라이언트 접속 해제");
        list.remove(session);
    }
}
```
* payload ?

페이로드란 전송되는 데이터를 의미한다.

데이터를 전송할 때, Header와 META 데이터, 에러 체크 비트 등과 같은 다양한 요소들을 함께 보내 데이터 전송 효율과 안정성을 높히게 된다. 

이때, 보내고자 하는 데이터 자체를 의미하는 것이 페이로드이다.

{
"status":
"from":"localhost",
"to":"http://melonicedlatte.com/chatroom/1",
"method":"GET",
**"data":{"message":"There is a cutty dog!"}**
}

3. WebSocket Config


https://dev-gorany.tistory.com/212

