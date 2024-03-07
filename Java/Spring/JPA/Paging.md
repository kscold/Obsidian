- [[데이터베이스(DataBase)]]에 저장된 데이터들을 페이지에 맞춰서 몇 개씩 뿌릴건지 알려주는 것이다.

- 예를 들어, [[데이터베이스(DataBase)]]에 저장된 데이터가 20개이고 프론트에서 1페이지에 5개씩 이라고 요청하면 백엔드에서 전체 DB에서 데이터를 앞에서부터 5개씩 분류하여 해당 페이지에 맞는 데이터를 넘겨준다.

## Pageable

- 따라서 페이징을 위해서 [[JPA(Java Persistence API)]]에서 Pageable이라는 [[객체(Object)]]를 제공하고있다.

![](https://blog.kakaocdn.net/dn/bbbpr4/btrOdgO1qhy/qPlSnT6xm83ifI5NrgTsy1/img.png)

- Pageable 이 Pagination 요청 정보를 담기위한 추상 [[인터페이스(Interface)]]라는 의미이다.
- 실제로 쓰기 위해서는 구현체가 필요하다.

#### class QPageReques

- [[QueryDSL]] 을 위한 Pageable 구현체이다.
#### class PageRequest

- 가장 기본적인 Pageable 구현체이다.
#### enum Unpaged

- pagination 정보가 없는 것을 표현하기위한 구현체. INSTANCE 를 가지고 있다.


- 이 중 가장 기본이 되는 PageRequest를 가장 많이 사용한다.(보통 Cusotom Query를 사용해서 같이 사용한다.)

## PageImpl

- Page의 구현체 PageImpl이다.

![](https://blog.kakaocdn.net/dn/20hJ1/btrOhX2lbiZ/ErWye4IMpBmEu380dO6ch0/img.png)

### List`<T>` content 

- 해당 페이지에 들어갈 내용이다.
- [[List]]이다.
### Pageable pageable 

- 요청한 페이지의 정보이다.
### long total

- 들어갈 내용들의 양이다.
- 총 데이터의 갯수이다.

- Postman 조회하면 다음과 같다.

1. 파라미터 정보 없이 요청한 경우이다.

![](https://blog.kakaocdn.net/dn/GrBTX/btrOigOxoLT/xbNYETupPxqfMvq8CjarCk/img.png)

- totalPages : 총 페이지 수
- totalElements : DB의 전체 데이터 갯수
- last : 마지막 페이지 인지
- size : 페이지 당 나타낼 수 있는 데이터의 갯수
- number : 현재 페이지의 번호
- sort 
    - Sorting에 대한 값을 설정하는 파라미터. 기본적으로 오름차순.
    - 표기는 정렬한 필드명 , 정렬기준(ex. sort=title,asc    sort=rank,desc) 
    - 현재는 설정이 되어있지 않아 sorted = false로 나와있다.
- numberOfElements : 실제 데이터의 갯수
- first : 첫번째 페이지 인지
- empty : 리스트가 비어있는지 여부

2. size = 2 로 보낸 경우이다. 
- ?size=2, 쿼리스트링 size가 2일 때이다.

![](https://blog.kakaocdn.net/dn/HFEbf/btrOeQwffzv/W3PcNqjo4clnHZnMNyefZ1/img.png)

- size가 2로 변경이 되어있고 첫번째 페이지만 출력이 된 걸 알수 있다.

## 예시

- 아래 예시는 [[리파지터리(Repository)]] 부분 구현 코드이다.

```java
public interface MeetingRepository extends JpaRepository<Meeting,Long> {
	Page<Meeting> findAllByOrderByCreatedAtDesc(Pageable pageable);
}
```

- 아래 예시는 [[컨트롤러(Controller)]] 부분 구현 코드이다.

```java
@GetMapping
public ResponseDto<Page<MeetingResponseDto>> getAllMeeting(@PageableDefault(size = 12) Pageable pageable) {
	return ResponseDto.success(meetingService.getAllMeeting(pageable));
}
```

- Pageable을 객체로 넘기면서 Repository에서 원하는 만큼의  Page를 반환 받는다.

- [[서비스(Service)]]도 변경해준다

```java
public Page<MeetingResponseDto> getAllMeeting(Pageable pageable) {
	
    Page<Meeting> meetingList = meetingRepository.findAllByOrderByCreatedAtDesc(pageable);
    List<MeetingResponseDto> meetingResponseDtoList = new ArrayList<>();
	
    for (Meeting meeting : meetingList) {
        MeetingResponseDto meetingResponseDto = new MeetingResponseDto(meeting);
        meetingResponseDtoList.add(meetingResponseDto);
    }
	
    return new PageImpl<>(meetingResponseDtoList, pageable, meetingList.getTotalElements());
}
```

- new PageImpl 구현체를 사용하여 Page를 반환해주면 된다.


## 페이징에서 마지막 페이지 번호를 찾는 알고리즘

- 예를 들어 현재 번호가 12면 페이지를 어떻게 찾아야할까?
- 페이지네이션은 10개씩이라고 가정한다.

1. 페이지의 마지막 번호를 기준으로 생각한다.
2. 12를 10.0으로 나눈다.
3. 1.2를 올림한다.
4. 2가 되었으니 여기에 10을 곱한다.
5. 이제 -9를 해주면 11부터 20 사이의 페이지라고 정의할 수 있다.