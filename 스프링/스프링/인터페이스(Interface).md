
Interface 파일의 경우 자바에서 사용되는 [[클래스(Class)]]의 기본 틀이라고 생각하면 된다.  다른 클래스를 작성할 때 기본이 되는 틀을 제공하고 다른 클래스 사이의 중간 매개 역할을 하는 일종의 [[추상 클래스(Abstract Class)]]와 유사한 역할을 한다.

즉 일종의 어떤한 설계도이다. 이러한 설계도에 우리는 맞추어서 규칙으로써 인터페이스에 맞추어 개발하고 그 규칙을 맞추어 개발하게 되면 우리는 내부 구현을 알 필요도 없이 그냥 인터페이스의 함수가 잘 동작할 것이라고 생각하고 호출하면 된다.

Interface는 오로지 [[추상 메서드(Abstract Method)]]와 상수만을 포함한다.

함수형 인터페이스 선언
[[@FunctionalInterface]] // 
interface Math {
    public int Calc(int first, int second);
}
