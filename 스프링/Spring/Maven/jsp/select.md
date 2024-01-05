
- 여러 개의 항목이 나타나는 목록 상자에서 항목을 선택하는 태그

- 시작 태그와 종료 태그가 있으며, 리스트 박스에 여러 항목을 추가 삽입하기 위해 반드시 option 태그를 포함해야 함

select 문법

```jsp
<label for="assNumber">음료</label>
  <select>
    <option value="밀크티">밀크티</option>
    <option value="라떼">라떼</option>
    <option value="아아">아아</option>
  </select>
```


### **select 문에서 특정 option 선택하기**

```jsp
<label for="assNumber">음료</label>
  <select>
    <option value="밀크티">밀크티</option>
    <option value="라떼" selected>라떼</option>
    <option value="아아">아아</option>
  </select>
```

결과 : **_selected_** 를 사용한 태그가 선택되어져 있는걸 볼 수 있습니다.

### **select 문에서 <c:forEach> 사용하여 서버에서 값 불러오기**

```jsp
 <label for="assNumber">음료</label>
 <select>
   <option value="">선택해주세요.</option>
     <c:forEach var="list" items="${result}">
     	<option value="${list.beverage}">${list.beverage}</option>
    </c:forEach>
 </select>
```

${result}에는 밀크티, 라떼, 아아가 들어있다.

### **select 문에서 <c:forEach> 와 <c:if> 사용하여 선택한값 체크하기**

```
 <label for="assNumber">음료</label>
 <select>
   <option value="">선택해주세요.</option>
   <c:forEach var="list" items="${result}">
 	  <option value="${list.beverage}" <c:if test ="${user.selectedBeberage eq list.beverage}">selected="selected"</c:if>>${list.beverage}</option>
   </c:forEach>
 </select>
```

${result}에는 밀크티, 라떼, 아아가 들어있습니다. ${user.selectedBeberage}에는 사용자가 선택한 음료가 들어있습니다. for문을 돌면서 일치하면 그것을 보여줍니다.

### **radio 태그에서 선택한값 체크하기**

마찬가지로 radio 태그에서 선택된 값은 selected 대신에 _**checked**_을 사용해주시면 됩니다.

```
<li class="<c:if test="${user.selectedBeberage eq '밀크티'}">active</c:if>">
밀크티
<input type="radio"  id="check_group_01" name="check_group" value="밀크티" <c:if test="${user.selectedBeberage eq '밀크티'}">checked</c:if>>
</li>

<li class="<c:if test="${user.selectedBeberage eq '라떼'}">active</c:if>">
라떼
<input type="radio"  id="check_group_02" name="check_group" value="라떼" <c:if test="${user.selectedBeberage eq '라떼'}">checked</c:if>>
</li>

<li class="<c:if test="${user.selectedBeberage eq '아아'}">active</c:if>">
아아
<input type="radio" id="check_group_03" name="check_group" value="아아" <c:if test="${user.selectedBeberage eq '아아'}">checked</c:if> >
</li>
```

**_class에서도 <c:if>_**문 사용가능함을 보여드리기위해 넣어보았습니다.

jsp에서 jstel을 활용한 select박스 selected, radio 박스 checked, 그리고 forEach, <c:if>문까지 알아보았습니다. 감사합니다.

출처: [https://bluemint.tistory.com/16](https://bluemint.tistory.com/16) [BLUEMINT:티스토리]