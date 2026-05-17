- [[JPA(Java Persistence API)]]에서 제공하는 객체지향쿼리인 JPQL(Java Persistence Query Language)을 통해 동적 [[쿼리(query)]]를 구성하면 코드가 굉장히 난잡해진다는 것을 느낄 수 있다.

- [[JPQL(Java Persitence Query Language)]]은 문자열을 사용한다.
- 문자열을 조건에 따라 이어붙이는 형식으로 구성하기 때문에 생기는 문제가 있다. 
- 문자열이기에 오타가 발생해도 컴파일 단계에서 에러를 잡아주지 못한다.(다만, NamedQuery를 사용하면 가능하다.)

- 또한 동적쿼리를 구성할 때, 중간중간 if문에 의해 문자열이 추가되기 때문에 가독성이 떨어진다.
- 따라서 쿼리를 체계적으로 관리하기 어렵다.

- QueryDSL은 위와 같은 문제를 해결하기 위해 만들어졌다. 
- Type-Safe한 [[쿼리(query)]]를 사용하기 위해 [[엔티티(Entity)]]와 매핑되는 정적 타입 QClass를 생성해 쿼리를 생성할 수 있게 만들었다. 
- 컴파일 단계에서 오류를 잡아낼 수 있고 [[메서드(Method)]] 체이닝을 통해 조건을 보다 쉽게 추가할 수 있다.
- 즉, 동적 쿼리를 작성할 때 용이해진다는 말이다.

- QueryDSL도 내부적으로는 [[JPQL(Java Persitence Query Language)]]를 구성해 [[쿼리(query)]]를 만들어낸다. 
- 다만 사용자들이 편하게 사용할 수 있게 추상화해놓은 JPQL 빌더라고 생각하면 된다.

## QueryDSL 메서드

### getQuerydsl().applyPagination() 

- 스프링 데이터가 제공하는 페이징([[Paging]])을 Querydsl로 편리하게 변환 가능하다.
- 단, Sort는 오류발생한다.
### where()

- where() [[메서드(Method)]]는 [[쿼리(query)]]의 조건을 지정하는데 사용한다.
### fetch()

- fetch() 메서드를 호출하여 쿼리를 실행하고 결과를 가져온다.
- 이 메서드는 목록 데이터를 가져오는데 사용된다.
### fetchCount()

- fetchCount() 메서드를 호출하여 쿼리의 결과 개수를 가져온다.
- 이 메서드는 페이지네이션([[Paging]])을 위해 전체 개수를 가져오 는데 사용된다.

## QueryDSL 설정

- QueryDSL를 외부로 빼서 확인하는 설정이다.
- 설정이후 build를 누르면 외부에 QClass가 생성된다.

```
// queryDSL 스프링 2.0 설정  
implementation"com.querydsl:querydsl-jpa"  
implementation"com.querydsl:querydsl-core"  
implementation"com.querydsl:querydsl-collections"  
annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa" // querydsl JPAAnnotation  
annotationProcessor "jakarta.annotation:jakarta.annotation-api" 
// java,lang.NoClassDefFoundError (javax.annotation.Generated) 대응 코드  
annotationProcessor "jakarta.persistence:jakarta.persistence-api" 
// java,lang.NoClassDefFoundError (javax.annotation.Entity) 대응 코드


// queryDSL 스프링 3.0 설정  
implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'  
annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"  
annotationProcessor "jakarta.annotation:jakarta.annotation-api"  
annotationProcessor "jakarta.persistence:jakarta.persistence-api"


// 인텔리제이와 jdk 차이에서 나는 오류를 최소화하기 위한 설정부(없어도 되나 빌드할 때 불편함을 최소화함)  
// Querydsl 설정부  
def generated = 'src/main/generated'  
  
tasks.withType(JavaCompile) {  
    options.getGeneratedSourceOutputDirectory().set(file(generated))  
}  
  
// java source set에 quertdsl QClass 위치 추가  
sourceSets {  
    main.java.srcDirs += [generated]  
}  
  
// gradle clean 시에 QClass 디렉토리 삭제  
clean{  
    delete file(generated)  
}
```

- build 폴더 안에 generated에 qeuryDSL를 설정하는 방법이다.
- 설정하고 other에 complieJava를 실행하면 QClass가 생성된다.

```
// 스프링 부트 3.0 설정
// 맨 위에 버전 설정
buildscript {  
    ext {  
        queryDslVersion = "5.0.0"  
    }  
}

// 의존성 설정
dependencies {
	
	annotationProcessor(  
	        "jakarta.persistence:jakarta.persistence-api",  
	        "jakarta.annotation:jakarta.annotation-api",  
	        "com.querydsl:querydsl-apt:${queryDslVersion}:jakarta"  
	)
	
}

// 마지막에 설정
compileJava.dependsOn('clean') // 컴파일할 때 다 지우고 새로 만들어 주는 설정
```