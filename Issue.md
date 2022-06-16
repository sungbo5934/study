
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
