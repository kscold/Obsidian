- [[class-validator]] 라이브러리를 사용하여 커스텀 유효성 검사기를 만드는 방법이다.

- [[class-validator]]는 내장된 다양한 [[데코레이터(Decorator)]]를 제공하지만, 특정 요구사항에 맞는 유효성 검사를 위해 [[커스텀 데코레이터(Custom Decorator)]]를 정의할 수 있다. 


## 커스텀 유효성 검사기 정의 단계

1. ValidatorConstraintInterface 인터페이스([[Interface]]) 구현
    먼저, 커스텀 유효성 검사기를 만들기 위해서는 ValidatorConstraintInterface를 구현하는 [[클래스(class)]]를 작성해야 합니다. 
    이 인터페이스([[Interface]])는 `validate` [[메서드(Method)]]를 필수로 구현해야 한다.

2. 유효성 검사를 위한 클래스 정의  
    [[클래스(class)]]에서 `validate` [[메서드(Method)]]를 통해 유효성 검사를 구현하고, 이 메서드에서 비즈니스 로직에 맞는 유효성 검사를 수행한다.
 
3. `defaultMessage` 메서드 구현
    유효성 검사가 실패할 경우 표시할 기본 오류 메시지를 `defaultMessage` 메서드에서 정의할 수 있다.
    이 메서드는 선택 사항이다.

3. `registerDecorator` 함수 사용하여 [[데코레이터(Decorator)]] 정의
    `registerDecorator`를 사용하여 [[커스텀 데코레이터(Custom Decorator)]]를 정의한다.
    [[데코레이터(Decorator)]]는 유효성 검사기를 적용할 [[클래스(class)]] 속성에 붙여 사용된다.


## 문법

```ts
@ValidatorConstraint()  
class validate클래스명 implements ValidatorConstraintInterface {  
    validate(  
        value: any,  
        validationArguments?: ValidationArguments,  
    ): Promise<boolean> | boolean {  
        return value.length > 4 && value.length < 8;  
    }  
	  
    defaultMessage(validationArguments?: ValidationArguments): string {  
        return '메세지';  
    }  
}  
  
function IsPasswordValid(validationOptions?: ValidationOptions) {  
    return function (object: object, propertyName: string) {  
        registerDecorator({  
            target: object.constructor, // 기본 속성  
            propertyName, // 기본 속성  
            options: validationOptions, // 기본 속성  
            validator: validate클래스명,  
        });  
    };  
}
```