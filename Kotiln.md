
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
      
  ## private val, val, var, const val
   * private val
      + getter/setter 를 모두 생성해주지 않아서 외부에서 접근 불가
   * val
      + setter 를 생성해주지 않아 외부에서 값 변경 불가
   * var
      + getter/setter 를 모두 생성해주므로 외부에서 자유롭게 활용가능
   * const val
      + 컴파일 시점에서 할당하며 기본 자료형에서만 사용가능
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

  ## 확장함수, 확장 프로퍼티
   * 어떤 클래스의 멤버 메서드인 것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수
      + ex: collection<T>.max(), String.split(varang..)
   * kotlin에서는 확장 함수를 통해 여러가지 java에서 제공하는 api말고 다양한 api 제공
   * 확장 함수를 오버라이딩 할 수 없음 ( 정적호출 )
   * 확장 프로퍼티는 위의 확장함수와 같은 개념이지만 선언시 get(), set()을 선언해서 사용
  
  ## 중위호출과 구조분해선언
   * 클래스의 멤버 호출 시 사용하는 점(.)을 생략하고 함수 이름 뒤에 소괄호를 생략해 직관적인 이름을 사용하여 표현하는 방법
      + ex: mapOf(1 to "one", 7 to "senver")에서 to
   * infix키워를 사용하요 선언
      + ex: infix fun Ayny.to(other: Any) = Pair(this, other)
   * 구조분해 선언이란 객체가 가지고 있는 여러 값을 분해해서 여러 변수에 한꺼번에 초기화하는 방법
   ```
  val address = Address("서울", "서대문구", "연희동", "불필요사항")
  val (city, gu, dong, _) = car
  * 불필요한 사항은 '_'을통해 대체가능
  >>> println("시 : ${city}, 구 : ${gu}, 동 : ${dong}")
   ```
  ## 로컬함수
   * 메소드내에서 반복되는 구문을 메소드내에 추가로 메소드를 만들어서 사용하는 방법
 ```
  fun sacvUser(user: User){
    fun validate(...){
      //valid logic  
    }
    validate(..)
    validate(..)
  }
 ```
  ## sealed clase
   * Super class(Sealed class)를 상속받는 Child 클래스의 종류 제한하는 특성을 갖고 있는 클래스
   * 같은 패키지의 자식 클래스만 상속가능 ( 모든 패키지에서 컴파일러가 검색하기에는 리소스낭비 )
   * 특징
```
  sealed class Person()

  class Arm : Person()
  class Face : Person()
  class Hand : Person()

  //위와 깉이 선언 시 

  fun getWhen(type: Person): String{
    when(type){
      is Arm -> "팔"
      is Face -> "얼굴"
      is Hand -> "손"
    //else -> Exception()
    //위와 같이 컴파일러가 이미 child클래스를 다 알 수 있기때문에 else를 작성할 필요가 없어짐
    }
  }
```

  ## 주생성자, 부생성자
   * 주생성자 : 주로 사용하는 간략한 생성자로, 클래스 본문 밖에서 정의
   * 부생성자 : 클래스 본문 안에서 정의
```
  // 주생성자 예시
  // 가장 간단한 방법
  // var/val를 붙이면 생성자가 되는 동시에 프로퍼티 (필드)가 생성됨
  class User(val nickname: String)
  
  // init 함수를 통한 초기화진행
  // data 클래스에서는 불가 (var/val 키워드 필수)
  // _nickname이라는 프로퍼티 (필드)는 생성되지 않음
  class User constructor(_nickname: String){
    val nickname: String
    init {
        nickname = _nickname
    }
  }
  
  class User constructor(_nickname: String){
    val nickname: _nickname
  }
  
  // 부생성자 예시
  // 주생성자 말고 다른 변수를 받아 프로퍼티에 대입하고 싶을때 사용함
  class User(val nickname: String){
    private val age:Int = 0
  
    consturctor(nickname: String, age: Int): this(nickname){
      this.age = age
    }
  }
  
```

  ## by (위임, delegate)
   * 어떤 클래스의 멤버를 참조 없이 호출하고 싶을 때 사용하는 선언 방식
   * 어떤 기능을 자신이 처리하지 않고 다른 객체에 위임을 시켜 그 객체가 일을 처리하도록 하는 것
   * 코틀린의 기본 라이브러리는 상속할 수 없도록 정의되어 있다. 실제로 List 등의 클래스는 모두 open이 아니다. 이렇게 함으로써 라이브러리의 무분별한 상속을 막을 수 있지만, 클래스의 기능을 확장하기 어려워지는 문제가 발생
```
  // 1
  interface Animal {
      fun eat()
  }
  // 2
  class Cat : Animal {
      override fun eat() {
          println("eat() at Cat")
      }
  }
  // 3
  val cat = Cat()
  // 4
  class Robot : Animal by cat

  1. 인터페이스 Animal을 정의
  2. 클래스 Cat을 선언하고, Animal을 구현하였다. Cat은 추상 클래스 또는 인터페이스가 아니므로 eat()을 정의함
  3. Cat의 객체 cat을 만듬
  4. 클래스 Robot을 선언하고, Animal을 구현하였다. Robot 역시 eat()을 정의해야 할 것 같지만, by 키워드를 사용하여 cat에게 정의를 위임
```
  
  ## companion
  ## object
  
  
