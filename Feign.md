
# Feign. 
  https://spring.io/projects/spring-cloud-openfeign.  
  https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/
  ## config
   * FeignAutoConfiguration.class에 의해 필요 bean들이 등록됨 bean 충돌 대비
      + @configuration 관련 https://blog.leocat.kr/notes/2019/03/27/feign-open-feign-configuration
      + @Scope("prototype")을 이용하여 충돌을 피할 수 있음
   * feign client 마다 따로 config class를 두는 방식으로 한다면 FeignClientFactoryBean.class 파일의 configureUsingConfiguration() 함수를 통해 custom하게 정의된 bean들이 등록됨
      + 해당 메소드안을 보면 사용자가 bean을 따로 만들지 않았으면 inner class인 **Builder** ( member 변수를 통해 feign인 어떤 bean을 사용하는지 파악할 수 있음)를 통해 default로 정의 
  ## client ( okhttp )
      // feign.okhttp.OkHttpClient.class 참조
  * okhttp의 경우 readtimeout, connectTimeoutMillis 등이 Feign.Builder 의 Request.Options()의 timeout 보다 우선 순위가 낮은걸 유의
  * okhttp는 단일 인스턴스로 모든 http 요청을해야한다. connection pool과 스레드 풀이 공유되니 때문에 재사용하기 좋다.  
      + 참고 : https://square.github.io/okhttp/4.x/okhttp/okhttp3/-ok-http-client )
      + 단일인스턴스라고 하지만 외부 네트워크 api 호출 시 okhttpclient의 디버그를 걸면 항상 hashcode(), 즉 인스턴스의 주소값이 다르다. 이유를 알아보자.. 
  * apache의 httpclient는 해당 클라이언트에서 사용할 수 있는 maxconnection을 설정할 수 있었지만 okhttp 같은경우는 maxIdleConnection, keepalive time 등만 지정한다? 아직찾지못했을수도?..  
      + 참고 : https://github.com/square/okhttp/issues/3907, https://stackoverflow.com/questions/46206267/okhttp-how-to-set-maximum-connection-pool-size-not-max-idle-connections
      + IdleConnection : 활성 상태이지만 장기간 동안 어느 쪽 장치에서도 데이터가 전송되지 않은 커넥션 , keepalive time : 해당 시간 동안 통신이 없다면 커넥션은 죽임
   * 위의 connection pool 관련 의문은 sync방법이지만 async 방식에서는 Dispatcher.class를 이용하여 maxrequest, per request, excuterservice(thread pool) 등을 지정할수 있다.(이 부분은 아직 실험 x)
  ## circuit breaker
   * Fallback
      + fallbackfactory를 통한 에러 시 기본값을 줄는 로직을 작성 시 fallback에서의 exception종류는 aop를 통해 받는 exception의종류가 다름
      + https://github.com/OpenFeign/feign/issues/386 를 참고하면 client 부분의 throw를 지정하면 랩핑된 UndeclaredThrowableException 이 오지않음
