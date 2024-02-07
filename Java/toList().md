- [[스트림(Stream)]]을 [[List]]로 변환해주는 메서드이다.

- JDK 16에서 기존의 collect(Collectors.toList()) 부분을 toList() 메서드 묶은 간결화 버전이 생겼다.
- 그러나 실질적으로 반환하는 구현체가 완전히 똑같지는 않기 때문에 [[collect()]]의 collect(Collectors.toList()) 구분을 사용하는 것이 좋다.(즉 11버전과 17버전 둘 다 collect(Collectors.toList()) 사용이 더 바람직하다.)

## 예시

```java
List<Article> articlesList = dtos.stream() // dtos 객체를 스트림으로 변환
        .map(dto -> dto.toEntity()) // 낱개의 dto 스트림 객체를 스트림 엔티티로 변환
        .collect(Collectors.toList()); // 스트림 엔티티로 변환된 객체를 리스트로 변환
```