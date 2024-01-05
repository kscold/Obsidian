Maven은 자바 프로젝트의 빌드(build)를 자동화 해주는 빌드 툴(build tool)이다.

즉, 자바 소스를 compile하고 package해서 deploy하는 일을 자동화 해주는 것이다.


**Maven이 참조하는 설정 파일**

Maven 전체를 보기보다 프로그래밍에 직접적인 연관이 있는 두 개의 설정파일을 알아보면 된다.

**1) settings.xml**

settings.xml은 maven tool 자체에 관련된 설정을 담당한다.

MAVEN_HOME/conf/ 아래에 있다. ( * MAVEN_HOME은 환경변수에 설정한 경로)

Maven 자체에 설정 값을 바꾸는 일은 일단 잘 없으므로 넘어가고 기획한대로 pom.xml을 살펴본다.

  

**2) [[pom.xml]]**

하나의 자바 프로젝트에 빌드 툴로 maven을 설정했다면, 프로젝트 최상위 디렉토리에 "pom.xml"이라는 파일이 생성되었을 것이다.

pom.xml은 POM(Project Object Model)을 설정하는 부분으로 프로젝트 내 빌드 옵션을 설정하는 부분이다.

꼭 pom.xml이라는 이름을 가진 파일이 아니라 다른 파일로 지정할 수도 있다. (mvn -f ooo.xml test)

그러나 maven의 원칙(습관에 의한 편의성?)으로 다른 개발자들이 헷갈릴 수 있으므로 그냥 pom.xml으로 쓰기를 권장한다.



**3) build 정보**  

build tool : maven의 핵심인 빌드와 관련된 정보를 설정할 수 있는 곳이다.

`<build>` 부분에서 설정할 수 있는 값들에 대해 설명하기 전에 "라이프 사이클(life-cycle"에 대해서 알 필요가 있다.

객체의 생명주기처럼 maven에는 라이프 사이클이 존재한다.

크게 default, clean, site 라이프 사이클로 나누고 세부적으로 페이즈(phase) 있다. 아래 그림 참조

![[Pasted image 20231012133632.png]]

메이븐의 모든 기능은 플러그인(plugin)을 기반으로 동작한다.

플러그인에서 실행할 수 있는 각각의 작업을 골(goal)이라하고 하나의 페이즈는 하나의 골과 연결되며, 하나의 플러그인에는 여러 개의 골이 있을 수 있다.

