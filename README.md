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
    * 블록킹 논블록킹
          
* Reactive

* Backpressure 
        
* functional programming
    * vs mvc controller
          
* reactive programming
    * mono
    * flux
        
* blockhound
    
