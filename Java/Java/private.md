- Java에서 기본적으로 [[접근제어자(Access modifier)]]를 명시하지 않으면 
- [[인스턴스 변수(instance variable)]]를 선언할 때 사용한다.(패키지 내에서만 접근 가능한 멤버로 간주한다.)
- 반면에 private의 경우 [[클래스(Class)]] 안에서만 사용 가능하다.

- private 멤버는 class 외부에서는 바로 접근할 수 없는 변수이다.

```java
public class PrivateMember {

    public static void main(String[] args) {

        // TODO Auto-generated method stub

        Car2 carObject = new Car2();

        // 값을 직접 대입할 수 없다.

        //carObject.fuel = 10;

        carObject.setFuel(10);

        System.out.println(carObject.getFuel());

        carObject.setFuel(-10);

        System.out.println(carObject.getFuel());

    }

}

class Car2 // private로 만들어서 fuel에 조건을 걸어
{

    private int fuel;

    void setFuel ( int inputFuel ) {

        if ( inputFuel > 0 && inputFuel <= 100) { // 이런식으로 조건을 걸어 실수를 방지

            this.fuel = inputFuel;

        }

        else {

            System.out.println("fuel 값이 올바르지 않습니다.");

        }

    }

    int getFuel () {

        return this.fuel;

    }

}
```

위처럼 [[Getter and Setter]] 함수에서는 Private 멤버에 제약조건을 걸어서 예기치 않은 값에 대해 문제를 방지할 수 있다.


따라서 클래스에 데이터(필드)와 기능(메소드)을 한 곳에 모아둔 다음 보호하고 싶은 멤버에 private 를 붙여 접근을 제한하는 기능을 일컬어 캡슐화라고 한다.

참고로 보통의 경우 필드(변수)는 private 로 함수(메소드)는 [[public]] 로 지정하곤 한다.

예시) private로 studentID를 받아 매서드를 통해서만 수정할 수 있음 
즉, 정보 은닉([[캡슐화(encapsulation)]])에 유용하다.

```java
package hiding;

public class Student { 
	private int studentID;
	public String studentName;
	public int grade;
	public String address;

	
	public int getStudentID() {
		return studentID;
	}
	public void setStudentID(int studentID) {
		if(studentID >32) {
			System.out.println("오류, 다시 입력하시오");
		}else {
			this.studentID = studentID;
		}
	}
}

package hiding;
 
public class StudentTest {
 
	public static void main(String[] args) {
		Student student = new Student();
		
		student.setStudentID(33);
	}
}

```

위의 방법을 사용하면  [[Getter and Setter]] 함수 안에 조건을 걸어 놓았으므로 실수를 줄일 수 있다.