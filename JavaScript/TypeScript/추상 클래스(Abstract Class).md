- 추상 메서드는 정의만 있을 뿐 몸체(body)가 구현되어 있지 않는다.
- 몸체는 추상 클래스를 상속하는 [[클래스(class)]]에서 해당 [[추상 메서드(Abstract Method)]]를 통해 필히 구현해야 한다.

- [[타입스크립트(TypeScript)]]에서 추상 클래스를 정의할 때는 [[클래스(class)]] 앞에 abstract라고 표기한다.
- 또한 [[추상 메서드(Abstract Method)]]를 정의할 때도 `abstract`를 메서드 이름 앞에 붙인다.

- 그리고 추상 클래스는 [[추상 메서드(Abstract Method)]] 뿐만 아니라, 실 사용이 가능한 [[메서드(Method)]]도 정의할 수 있다.

- 추상 클래스를 상속하는 [[클래스(class)]]를 통해 생성된 [[인스턴스(Instance)]]는 이 [[메서드(Method)]]([[메서드(Method)]], [[추상 메서드(Abstract Method)]])를 사용할 수 있다.

- 추상 클래스는 말 그대로 추상이므로 [[클래스(class)]]와 달리 [[인스턴스(Instance)]]를 생성하지 않는다.
- 생성 구문을 사용하면 오류가 발생한다.


## 추상 클래스 선언 및 사용

```ts
// 추상 클래스
abstract class Project {
	
	public project_name: string | null = null;
	private budget: number = 2000000000; // 예산
	
	// 추상 메서드 정의
	public abstract changeProjectName(name: string): void;
	
	// 실제 메서드 정의
	public calcBudget(): number {
	    return this.budget * 2;
	}
}

// [오류]
// [ts] 추상 클래스의 인스턴스를 만들 수 없습니다.
// constructor Project(): Project
let new_project = new Project();
```

- 아래 코드인 Project 추상 클래스를 상속 받은 WebProject 클래스는 추상 클래스 내에 정의된 추상 메서드를 반드시 구현해야 한다.
- 구현하지 않을 경우 다음과 같은 오류 메시지를 출력한다.

```ts
// 클래스 ⟸ 추상 클래스 상속
class WebProject extends Project {
  // [오류]
  // [ts] 비추상 클래스 'WebProject'은(는) 'Project' 클래스에서 상속된
  // 추상 멤버 'changeProjectName'을(를) 구현하지 않습니다.
  // class WebProject
}
```

- 그러므로 추상 클래스를 상속하는 [[클래스(class)]]는 추상 클래스에 정의된 [[메서드(Method)]]를 반드시 구현해야 한다.

```ts
class WebProject extends Project {
	// 추상 클래스에 정의된 추상 메서드 구현
	changeProjectName(name:string): void {
	    this.project_name = name;
	}
}


/* 인스턴스 생성 ------------------------------------------------ */
let new_project = new WebProject();

console.log(new_project.project_name); // null

new_project.changeProjectName('CJ 올리브 네트웍스 웹사이트 개편');

console.log(new_project.project_name); // 'CJ 올리브 네트웍스 웹사이트 개편'
```