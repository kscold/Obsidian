Optional<객체> findById(Long id)  | findByName(String name)

이런 Optional 형태의 매소드들은 주어진 Id나 name에 해당하는 객체를 찾아 반환한다. 만약 해당 ID의 멤버가 존재하지 않으면, 비어있는 Optional 객체가 반환된다.