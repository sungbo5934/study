
##feign
  1. client ( okhttp )
      - // feign.okhttp.OkHttpClient 참조
      - okhttp의 경우 readtimeout, connectTimeoutMillis 등이 Feign.Builder 의 Request.Options()의 timeout 보다 우선 순위가 낮음 Request.Op
      - FeignAutoConfiguration.class에 의해 필요 bean들이 등록됨 bean 충돌 대비 (@configuration 관련 https://blog.leocat.kr/notes/2019/03/27/feign-open-feign-configuration)
