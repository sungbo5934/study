
# Kotlin

  ## Stream vs Sequence vs Collections
    https://bcp0109.tistory.com/359
   * Lazy Evaluation (지연 평가) : 체이닝된 함수에서 마지막 조건 즉 최후의 결과값을 어떤것을 원하는지 파악 후에 로직이 돌아감, 따라서 성능이 올라감
   * Eager Evaluation (조급한 평가) : 체이닝된 함수에서 순차적으로 수식의 조건을 확인하면서 마지막 로직까지 파악함, 배열의 길이가 길어지면 불필요한 작업을 하게됨 
   * 하지만 무조건 Lazy Evaluation 방식이 좋은건 아님 (오버헤드 발생, 적은 수의 경우 큰차이없음) -> https://kotlinlang.org/docs/sequences.html
   > So, the sequences let you avoid building results of intermediate steps, therefore improving the performance of the whole collection processing chain. However, the lazy nature of sequences adds some overhead which may be significant when processing smaller collections or doing simpler computations. Hence, you should consider both Sequence and Iterable and decide which one is better for your case.
   
   ### Stream
   * Eager Evaluation 방식
   ### Sequence
   * Lazy Evaluation 방식
   ### Collections 
   * Lazy Evaluation 방식
   
  ## foreach문 continue, break
   * run 블럭
   * return @foreach

  ## coroutine
  

  ## image
   * BufferedImage
   > TYPE_3BYTE_BGR 스타일 BGR 색상 모델에 해당하는 8비트 RGB 색상 구성 요소가 있는 이미지를 나타냅니다. Blue, Green, Red 색상은 3바이트에 저장됩니다.
     TYPE_4BYTE_ABGR 3바이트 및 1바이트 알파에 저장된 파란색, 녹색 및 빨간색 색상이 있는 8비트 RGBA 색상 구성 요소가 있는 이미지를 나타냅니다.
     TYPE_4BYTE_ABGR_PRE 3바이트 및 1바이트 알파에 저장된 파란색, 녹색 및 빨간색 색상이 있는 8비트 RGBA 색상 구성 요소가 있는 이미지를 나타냅니다.
     TYPE_BYTE_BINARY 불투명한 바이트 패킹된 1, 2 또는 4비트 이미지를 나타냅니다.
     TYPE_BYTE_GRAY 인덱싱되지 않은 부호 없는 바이트 회색조 이미지를 나타냅니다.
     TYPE_BYTE_INDEXED 인덱싱된 바이트 이미지를 나타냅니다.
     TYPE_CUSTOM 이미지 유형이 인식되지 않으므로 사용자 정의된 이미지여야 합니다.
     TYPE_INT_ARGB 정수 픽셀로 압축된 8비트 RGBA 색상 구성 요소가 있는 이미지를 나타냅니다.
      static int	TYPE_INT_ARGB_PRE
      정수 픽셀로 압축된 8비트 RGBA 색상 구성 요소가 있는 이미지를 나타냅니다.
      static int	TYPE_INT_BGR
      Windows 또는 Solaris 스타일의 BGR 색상 모델에 해당하는 8비트 RGB 색상 구성 요소가 있는 이미지를 나타내며 Blue, Green 및 Red 색상이 정수 픽셀로 채워집니다.
      static int	TYPE_INT_RGB
      정수 픽셀로 압축된 8비트 RGB 색상 구성 요소가 있는 이미지를 나타냅니다.
      static int	TYPE_USHORT_555_RGB
      알파가 없는 5-5-5 RGB 색상 구성 요소(5비트 빨간색, 5비트 녹색, 5비트 파란색)가 있는 이미지를 나타냅니다.
      static int	TYPE_USHORT_565_RGB
      알파가 없는 5-6-5 RGB 색상 구성 요소(5비트 빨간색, 6비트 녹색, 5비트 파란색)가 있는 이미지를 나타냅니다.
      static int	TYPE_USHORT_GRAY
      인덱싱되지 않은 서명되지 않은 짧은 회색조 이미지를 나타냅니다.
      
  ## private val, val, var
   * private val
      + getter/setter 를 모두 생성해주지 않아서 외부에서 접근 불가
   * val
      + setter 를 생성해주지 않아 외부에서 값 변경 불가
   * var
      + getter/setter 를 모두 생성해주므로 외부에서 자유롭게 활용가능
   * getter/setter custom
   ```
   class Rectangle {
    var width = 10
        set(value) {
            field = value / 2
        }
    var height = 10
        set(value) {
            field = value / 2
        }
    var area: Int = 0
        get() = width * height
    }
   ```
   위와 같이 사용 가능하지만 주성생 field에서는 사용 불가    
