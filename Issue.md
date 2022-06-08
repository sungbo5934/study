
## Feign
  # 1. client ( okhttp )
      // feign.okhttp.OkHttpClient.class 참조
  * okhttp의 경우 readtimeout, connectTimeoutMillis 등이 Feign.Builder 의 Request.Options()의 timeout 보다 우선 순위가 낮음 Request.Op
  * FeignAutoConfiguration.class에 의해 필요 bean들이 등록됨 bean 충돌 대비 (@configuration 관련 https://blog.leocat.kr/notes/2019/03/27/feign-open-feign-configuration)
  + okhttp는 단일 인스턴스로 모든 http 요청을해야한다. connection pool과 스레드 풀이 공유되니 때문에 재사용하기 좋다.
    + apache의 httpclient는 해당 클라이언트에서 사용할 수 있는 maxconnection을 설정할 수 있었지만 okhttp 같은경우는 maxIdleConnection, keepalive time 등만 지정한다? ( 아직찾지못했을수도)
    ( 참고 : https://github.com/square/okhttp/issues/3907, https://stackoverflow.com/questions/46206267/okhttp-how-to-set-maximum-connection-pool-size-not-max-idle-connections)
    ( * IdleConnection : 활성 상태이지만 장기간 동안 어느 쪽 장치에서도 데이터가 전송되지 않은 커넥션 , keepalive time : 해당 시간 동안 통신이 없다면 커넥션은 죽인다. )
    + 단일인스턴스라고 하지만 서로 다른 request마다 excute하는 부분의 httpclient 주소값을 보면 서로다르다... 이유가 멀까?...
    + 위의 첫번쨰 의문은 sync방법이지만 async 방식에서는 Dispatcher.class를 이용하여 maxrequest, per request, excuterservice(thread pool) 등을 지정할수 있다.(이 부분은 아직 실험 x)
