# JVM(Java Virtual Machine)



#### JVM이란?

- 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것
- 자바와 OS 사이의 중재자 역활 -> OS에 구애받지 않고 자바 재사용 가능
- 메모리관리, Garbage Collection
- 스택기반 가상머신



#### 자바 프로그램 실행 과정

1. 프로그램 실행시 JVM은 OS로부터 프로그램이 필요로 하는 메모리 할당 받음

   - JVM은 메모리를 용도에 따라 여러 영역으로 나누어 관리

2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)[^1]

   로 변환

3. 클래스 로더를 통해 class 파일들을 JVM으로 로딩

4. 로딩된 class파일들은 Execution engine을 통해 해석

5. 해석된 바이트 코드는 Runtime Data Areas에 배치되어 실질적인 수행이 이루어 짐

   => 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization과 같은 GC같은 관리작업 수행



![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile23.uf.tistory.com%2Fimage%2F25616D45576B854C3FEAFB)



#### JVM 구성요소



##### Class Loader (클래스 로더)

- JVM내로 클래스(.class 파일)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈
- Runtime시 동적으로 클래스를 로드
- jar파일 내 저장된 클래스들을 JVM위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제



##### Execution Engine(실행 엔진)

- 클래스를 실행시키는 역활
- JVM내의 런타임 데이터 영역에 배치(클래스로더) 된 바이트코드를 실행
- 바이트코드를 실제 JVM내부에서 기계가 실행할 수 있는 형태로 변경



##### Interpreter(인터프리터)

- 자바 바이트 코드를 명령어 단위로 읽어서 실행
- 한 줄 씩 수행하므로 느림



##### JIT(Just - In - Time)

- 인터프리터 방식 단점 보완
- 인터프리터 방식으로 실행하다 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경, , 이후 네이티브 코드로 직접 실행
- 네이티브 코드는 캐시에 보관 :point_right: 한번 컴파일된 코드는 빠르게 수행



##### Garbage Collector 

- GC 수행 모듈



#### Runtime Data Area

- 프로그램 수행을 위해 OS에서 할당받은 메모리 공간

![](https://t1.daumcdn.net/cfile/tistory/992EE9465D08E9B903)

##### PC Register

- Thread 시작시 생성, Thread 마다 하나씩 존재
- 어떤 부분을 어떤 명령으로 실행해야할 지에 대한 기록
- 현재 수행중인 JVM 명령의 주소 가짐



##### JVM 스택 영역

- 프로그램 실행과정에서 임시로 할당 후 Method 호출 종료 후 소멸되는 특성의 데이터 저장 위한 영역
- 변수, 임시 데이터, Thread, Method 정보 저장



##### Native Method Stack

- 실제 실행 가능한 기계어로 작성된 프로그램을 실행시키는 영역
- Java가 아닌 다른 언어로 작성된 코드를 위한 공간



##### Method Area(= Class Area, Static Area)

- 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간
- Runtime Constant Pool 이라는 별도의 관리 영역도 존재 
  - 상수 자료형을 저장하여 참조, 중복을 막음



##### Heap

- 객체를 저장하는 가상의 메모리 공간
- new 연산자로 생성된 객체와 배열 저장

![](https://lh3.googleusercontent.com/e2vMJ_Spk2VKm0RzhPv7Uin-UPgG0dGfPg-JwIYEHLFISwspjdHeAwtzhOdpEsjB6CrEXzVkE1XVHiiExdlBrnxkpOCgkQjRF1M6pn9kI2Dv2cHPy8UDB7EfEXdW53ME8x4wvqFd)

- Permanent Generation
  - 생성된 객체들의 정보의 주소값이 저장
  - Class Loader에 의해 load되는 Class, Method 등에 대한 Meta 정보 저장

- Young Generation

  - Eden - 객체들이 최초로 생성되는 공간
  - Survivor 0 / 1 - Eden에서 참조되는 객체들이 저장되는 공간

- Old Generation

  - Young 영역에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간

    

[^1]: 자바 가상 머신이 이해가능한 언어로 변환된 자바 소스코드