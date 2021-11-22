# study

## 1. WebFlux
* Spring Boot 2 ( Spring 5 )
![image](https://user-images.githubusercontent.com/33863965/142557282-b423c3c8-2c60-4edd-93fc-51b190db826f.png)

* 사용이유 ( 제한적 )
    * 일반적인 SpringMVC는 동기 방식을 사용하며 1개의 Request의 보통 1개의 Thread가 생성되어 작업이 이루어지며 이때 대외 연계 시스템으로 보낸 호출 Thread가
대외 연계 시스템 장애로 교착 및 블락이 걸렸을때 해당 Thread가 해제되지 못하고 추가적으로 동일한 작업의 요청이 들어와 모든 TreadPool이 꽉차 당사 시스템에도 영향을 주게 된다.
따라서 EventLoop를 통해 non-blocking 방식의 WebFlux를 사용함으로써 하나의 Thread에 문제가 발생하여도 다른 Thread 및 시스템에 영향을 주지 않는 즉 독립적이게 될 수 있다.
    
* vs SpringMVC
    * 동기 비동기
      * 동기식 호출은 특정 서비스를 호출하면 완료될 때 까지 그 응답을 기다리는 방식이다. 함수의 결과를 호출한 쪽에서 처리하므로 쉬운 디버깅과, 직관적인 흐름 추적이 가능하다는 장점이 있다.
      * 비동기식 호출은 서비스를 호출한 후 즉시 응답을 받고, 다른 작업을 하다가 처리가 완료되었는지 확인하여 결과를 받는 방식이다. 즉, 함수의 결과를 호출한 쪽에서 처리하지 않는다. 동기식 호출은 서비스가 종료될 때까지 호출자가 마냥 기다려야 하므로 컴퓨팅 자원을 비효율적으로 사용할 수밖에 없지만, 비동기식 호출은 요청 결과를 기다리는 시간에 다른 작업을 수행할 수 있으므로 효율적으로 자원을 사용할 수 있다.
    * 블록킹 논블록킹
      * 소켓의 동작 방식은 블로킹(Blocking) 모드와 논블로킹(Non-Blocking) 모드로 나뉜다. 블로킹은 요청한 작업이 성공하거나 에러가 발생하기 전까지는 응답을 돌려주지 않는 것을 말하며 논블로킹은 요청한 작업의 성공 여부와 상관없이 바로 결과를 돌려주는 것을 말한다. 이때 요청의 응답값에 의해서 에러나 성공 여부를 판단한다.
          
* Reactive

* Backpressure 
        
* functional programming
    * vs mvc controller
          
* reactive programming
    * mono
    * flux
        
* blockhound


## 2. Spring Cloud Gateway
