- Swift에서 [[get]], [[set]]은 getter와 setter와 비슷하다. 
- 다만 해당 [[프로퍼티(Property)]]에 직접 붙히는 방식이라는게 조금 특이하다.

- get과 set을 사용하는 경우
1. 프로퍼티에 값이 할당 될 때 적절한 값인지 검증하기 위해 사용한다.
2. 다른 프로퍼티값에 의존적인 프로퍼티를 관리 할 때 사용한다.
3. 프로퍼티를 private하게 사용하기 위해서 사용한다.

밑에와 같은 코드를 작성할 시에  Xcode가 경고를 준다.
그 이유는 get과 set은 해당 프로퍼티에 직접 붙어있기 때문에 위와 같이 get{}, set{}에서 myProperty에 접근하면 recursive하게 자기 자신의 get, set을 호출하게 되므로 이렇게 사용하면 안된다.
일반적으로 get, set은 아래처럼 실제 값을 저장 할 backing storage가 필요하다. 

```swift
var myProperty: Int {  
   get {  
      return myProperty  
   }  
   set(newVal) {  
      myProperty = newVal  
   }  
}
print(myProperty) // myProperty 출력  
myProperty = 123 // newVal값은 123
```


그래서 get, set의 기본 syntax는 아래와 같이 외부에 [[저장 프로퍼티(Stored Properties)]]를 선하 myProperty의 값에 접근하거나 새로운 값을 할당하면 실제로 값이 저장되는 곳은 myProperty가 아닌 `_myProperty`이다. 즉 myProperty는 `_myProperty`의 interface역할을 한다.

밑에 코드를 제대로 활용을한 예시
```swift
var _myProperty: Int // 저장 프로퍼티 선언

var myProperty: Int {  
	get {  
		return _myProperty  
	}
	
	set(newVal) {  
		_myProperty = newVal  
	}  
}
```


- 프로퍼티에 값이 할당 될 때 적절한 값인지 검증하기 위해 사용할때
 
Company 클래스가 있고 그 안에 직원수를 나타내는 members 프로퍼티가 있다. 직원 수를 나타내는 members 변수는 Int 타입으로 선언 할 수 있다. 그런데 직원 수가 음수가 될 수는 없으니 members 값을 바꿀 때는 검증 할 필요가 있다.

```swift
class Company {  
	var _members:Int = 5 // 저장 프로퍼티
	
	var members:Int {  
	    get {  
		    return _members  
	    }
	    
	    set (newVal) {  
		    if (newVal < 1){  
		        print(“직원수는 한명보다 작을 수 없습니다.”)  
	        } else {
		        _members = newVal  
	        }  
	    }  
	}  
}
```

set{}의 newVal 값을 사용하여 프로퍼티의 값이 바뀌기전에 검증(validation) 할 수 있다.  

-  다른 프로퍼티값에 의존적인 프로퍼티를 관리 할 때

회식비를 의미하는 teamDinnerCost 프로퍼티가 추가 되었다. 
회식비는 직원 수에 비례하기 때문에 `_members` 프로퍼티에 의존이다. 
teamDinnerCost 프로퍼티에 get을 사용하면 직원수에 비례하여 계산된 회식비를 반환하도록 할 수 있다. 
이렇게 할 경우 teamDinnerCost의 값을 따로 신경쓰지 않아도 된다. 
또한 get{}만을 구현했으므로 외부에서 teamDinnerCost의 값을 변경하려고 하면 에러가 발생한다.
따라서 회식비는 직원수에 비례하여 정해진 값 만을 반환한다.

```swift
class Company {  
	var _members:Int = 5  
	var members:Int {  
		get {  
			return _members  
		} 
		set (newVal) {
			if (newVal < 1){  
				print(“직원수는 한명보다 작을 수 없습니다.”) 
			}else{ 
				_members = newVal 
			} 
		}
	}   
	
	var teamDinnerCost:Int {  
		get { 
			return _members * 100000 
		} 
	}
}
```

마지막으로 프로퍼티를 private하게 사용하기 위해 get, set을 쓸 수 있다. 
이 때는 직접적으로 get, set을 쓰지는 않습니다. 