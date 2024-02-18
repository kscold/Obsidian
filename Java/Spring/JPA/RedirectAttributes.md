- RedirectAttributes [[객체(Object)]] 타입의 매개변수 타입이다.
- RedirectAttributes [[객체(Object)]]를 사용해서 [[Redirect]] 페이지에서 사용할 데이터를 남길 수 있다.
- RedirectAttributes를 사용하려면, [[CrudRepository]]의 [[메서드(Method)]]의 매개변수로 받아와야 한다.


## 메서드
### addFlashAttribute()

- RedirectAttributes 객체 안에 정의되어 있는 메서드로 첫번째 매개변수에는 key를 두번째 매개변수에는 value를 넣어서 데이터를 이동시킨다.

```java
@GetMapping("/articles/{id}/delete")  
public String delete(@PathVariable Long id, RedirectAttributes rttr) {
	// RedirectAttributes 객체 이름을 rttr로 정의
    log.info("삭제 요청이 들어왔습니다!!");  
    // 1. 삭제할 대상 가져오기  
    Article target = articleRepository.findById(id).orElse(null);  
    log.info(target.toString());  
    // 2. 대상 엔티티 삭제하기  
    if (target != null) {  
        articleRepository.delete(target);  
        rttr.addFlashAttribute("msg", "삭제됐습니다!"); 
        // 리다이렉트시 msg라는 문자열 변수에 "삭제되었습니다!"를 넣어서 전달
    }  
  
    // 3. 결과 페이지로 리다이렉트하기  
    return "redirect:/articles";  
}
```

- 위에서 만든 msg 변수는 [[mustache]] 문법인 [[{{변수}}]]를 사용하여 [[스코프(Scope)]] 동안 사용할 수 있다.