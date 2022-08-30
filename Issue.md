
# Issue. 
  ## Spring Cloud Config
    https://cloud.spring.io/spring-cloud-config/reference/html/#_redis_backend
   * Threshold ( 변동 가능 )의 변경이 필요할때 재배포 없이 적용하기 위해 공부진행
   ### 특징
   *  설정 정보(application.yml) 를 외부에 보관할 수 있도록 지원해주는 서버
   *  위의 특징덕분에 서버를 재배포 하지 않고 클라이언트 서버들은 설정 파일의 변경사항을 반영
   ### 구조
   ![image](https://user-images.githubusercontent.com/33863965/174000923-51863060-071d-4ba0-892c-5f02292e4bf9.png)
   * 위의 그림에서는 설정 파일을 관리하는 point가 git이지만 redis, s3, jdbc(db) 등 다양한 저장소로 관리할 수 있음, 위의 공식 가이드 문서 확인
   * 순서
      + 사용자(admin)가 git, redis등 저장소에 설정파일을 push 및 변경
      + client 서버에서 config 서버로 필요한 설정값 요청 **( /actuator/refresh API )**
      + config 서버는 요청을 받으면 서버 기동시 등록해둔 url 및 endpoint로 해당 요청에 맞는 설정파일을 읽어 응답
      + client 서버는 응답받은 값으로 재기동 없이 변경 ( ex: @value 를 사용하여 변수 할당 시 )
    
   ### 고려사항
   * 현재에도 서버에 api 호출을 통해 threshold를 변경하고 있는데 보안상 좋지 않아 중앙에서 관리 해주는 point가 필요했지만 cloud config 역시 중간에 Spring Cloud Bus, rabbitMQ를 통해야만 **/actuator/refresh API** 호출 없이 실시간 변경이 가능

  ## RequestContextHolder
    https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/request/RequestContextHolder.html
   * 코루틴 ( 별도 스레드 )을 통한 현재 request 객체의 property를 이용 시 다수의 트레픽이 발생하면 NPE 발생으로 인해 공부진행
   ### 특징
   *  Spring에서 전역으로 Request에 대한 정보를 가져오고자 할 때 사용하는 유틸성 클래스
   *  Controller에서 HttpServletRequest 생성자 주입을 통한 request 객체를 받아오는게아니라 Business Layer나 다른 쪽에서 쉽게 request정보를 가저올 수 있음
   *  ThreadLocal의 값이므로 다른 쓰레드(new Thread, 혹은 executor를 사용한 ThreadPool에서의 참조 등) 에서는 RequestContextHolder 의 Request값을 꺼내 쓸 수 없음
   *  HttpRequest가 오는 시점에 Servlet이 생성될 때에 초기화가 되어지고 Business Layer를 거친 뒤 Servlet 이 destroy될 때 clean 
   
   ### 고려사항
   *  RequestContextHolder를 통해 얻어온 request 및 response를 다른 스레드에서 사용할 때에는 NPE가 발생할 수 있으므로 방어로직 및 메인 스레드에서 처리 후 넘겨주는 방식을 택해야함
   *  RequestContextHolder의 currentrequestattributes()는 null일때 exception을 thorw하므로 보다 안전하게 사용가능
   *  다수의 트레픽 (여러 스레드)이 발생 시 동기화 부분을 대비하여 jmeter 또는 jUnit 등으로 테스트 후에 운영배포를 진행해야함
  
  ## Feign Retry
   * Log적재 시스템의 호출 실패 및 적재 실패시 해당 로그가 유실이 됨을 대응하고자 공부진행
   ### 특징
   *  connection 을 가지고 오지 못했다거나, httpStatus code 가 0 이하인 invalid 한 status code 값이어야 RetryableException 이 발생
  
  ## Content-Type ( MultipartFile )
   * 이미지를 처리하는 시스템에서 이미지타입 (jpeg, png..)가 중요함
   * 상대방이 파일을 보낼 떄 Content-Type을 몰라 임의의 값을 줄 수가 있다. 이떄 실제 파일은 json, image/png 등이여도 application/octet-stream 등으로 와도 에러가 발생되지 않음
     따라서 상대방이 준 Content-Type을 신뢰하고 그걸 이용하여 사용하면 안됨
      + 방법 1 : 직접 그 파일을 read하여 Type을 알아내는 방법
  
  ## ImageIO
   * ImageIO에서 write시 원래의 format을 알지 않고 임의로 할 시 이미지가 열리지 않는 에러 발생
      + 참고 : https://stackoverflow.com/questions/11425521/how-to-get-the-formatexjpen-png-gif-of-image-file-bufferedimage-in-java
   * ImageIO를 통한 read()시 깨진 이미지나 읽지 못하는 이미지타입을 방어하는 로직이 없으면 안됨
   * ImageIO는 java에서 업데이틑 하고 있지 않으므로 다양한 구현체(TwelveMonkeys, JAI 등)를 이용하여 구현할 수 있음
   * BufferedImage 및 Graphics2D 객체 사용시 flush(), dispose()를 꼭 해주어야함 

  ## @valid
   * Kotlin에서는 spring valid 패키지내에있는 어노테이션 ( @notempty, @notnull..)등을 사용시 생성자 field에 적어 놓으면 작동하지 않음
      + 생성자 field에 어노테이션 적용히 컴파일된 소스에는 생성자 쪽에 어노테이션 쪽에 붙기 때문에 작동이 안됨
      + 생성자 field가 아닌 클래스 내에 field 위에 붙이면 정상 작동
      + 이를 방지하기 위해 @field:{valid어노테이션} 방식으로 작성 시 해당 부분을 생성자 field가 아닌 실제 field로 인식하여 정상 

  ## Spring
   * multipart
      + 상대방이 동일 변수명에 대해서 multipartfile 타입과 다른 타입(string, int ..)을 리스트 및 여러개 주었을때 HandlerMethodArgumentResolver를 구현한 ModelAttributeMethodProcessor의 constructAttribute()메소드 에서 해당 dto의 field로 타입미스 에러가 나면서 주입을 못하게되는 오류 발생, 또한 obejct형식으로 field을 선언 시 멀티파트 컨버터가 string 파람 컨버터보다 뒤에 있기에 덮어 쓰여지게되는 현상 발생

  ## Redis
   * redisson
      + pub/sub 시 publish하는 서버에서 serializable을 implement한 객체를 그대로 redis에 호출 시 sub하는 서버에서는 같은 코덱은 쓰더라도 동일 패키지에 class가 존재해야함
      + codec을 별도로 사용시 bean에 등록하여 사용해야함
      + 직렬화 및 역직렬화시 동일 패키지 및 동일 필드 구성에대한이유 -> https://techblog.woowahan.com/2551/
  
  ## 비동기 / 논블럭킹
   * webclient
      + 비동기 / 논블럭킹을 지원해주는 client이며 Resttemplate의 deprecated로 인한 대안
      + 비동기 / 논블럭킹을 스레드 갯수관점에서 보지말고 스레드 효율관점에서 보자
      + 메인 스레드 즉 기본 스레드의 블럭킹으로 인한 스레드 유휴시간을 줄이면서 그로 인해 해당 스레드는 스레드풀에 반납되어 더 많은 리퀘스트를 받을 수 있음
      + 비동기를 하기위해선 당연히 추가적인 스레드가 필요로함, 그 스레드 갯수를 줄이기 위해서 논블럭킹을 사용한다는 생각보단 쉬는 스레드를 줄인다는 관점으로 보면 편리
      + webclient와 코루틴을 통한 테스트 시 코루틴에서 사용한 스레드를 스레드 풀을 반납하였지만 갯수가 줄어들지않는 이유는 최초에 코루틴 (디스패처/io)을 생성시 스레드 3개가 미리 활성화 시켜놓으므로 4개이상의 코루틴 생성 시 스레드 갯수 이점을 확보 할 수 있음

  ## Serialize / Deserialize
   * jackson
      + data class 에서 상대방이 대문자로 보낼 시 해당 어노테이션을 붙이면 자동으로 파싱해서 deserialize를 해줌 ( @JsonNaming(value = PropertyNamingStrategies.UpperSnakeCaseStrategy::class) )
