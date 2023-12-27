스프링 return 값반환 때는 JsonConverter와 StringConverter가 있는데 여기서는 JsonConverter가 이용되는 것임

mvc같은 방식과 다르게 html를 주는 것이 아니라(JsonConverter) @GetMapping() 함수를 사용할 때, @ResponseBody를 설정하고 함수를 만들면 return 값을 줄 때 html이 같이 나오는 view가 작동되는 것이 아니라 정말 딱 그 값만 반환이 됨(StringConverter)

더 자세한 @ResponseBody의 작동 방법

HTTP의 BODY에 문자 내용을 직접 반환

viewResolver 대신에 HttpMessageConverter가 동작

기본 문자처리 StringHttpMessag

eConverter

기본 객체처리 MappingJackson2HttpMessageConverter

byre 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

-> 이 과정은 실무에서도 거의 건들지않고 그대로 사용을 함