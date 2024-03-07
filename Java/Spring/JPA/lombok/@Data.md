- @Data [[어노테이션(Annotation)]]은 [[롬복(lombok)]]의 [[@Getter]]  / @Setter, [[@ToString]], @EqualsAndHashCode와 @RequiredArgsConstructor 를 합쳐놓은 종합 선물 세트라고 할 수 있다. 

- POJO(Plain Old Java Objects)와 [[Bean]]과 관련된 모든 보일러플레이트(boilerplate = 재사용 가능한 코드)를 생성한다.

- class의 모든 필드에 대한 getter, setter, toString, equals와 같은 함수들 말이다.

```java
// lombok annotation 사용

@Data
public class Example {
    private String name;
    private double score;
}


// lombok annotation 미사용

public class Example {
    private String name;
    private double score;
	
    public DataExample(String name) {
        this.name = name;
    }
	  
    public String getName() {
        return this.name;
    }
	  
    public void setScore(double score) {
        this.score = score;
    }
	  
    public double getScore() {
        return this.score;
    }
	  
    @Override public String toString() {
        return "DataExample(" + this.getName() + ", " + this.getAge() + ", " + this.getScore() + ", " + Arrays.deepToString(this.getTags()) + ")";
    }
  
  protected boolean canEqual(Object other) {
    return other instanceof DataExample;
  }
  
  @Override public boolean equals(Object o) {
    if (o == this) return true;
    if (!(o instanceof DataExample)) return false;
    DataExample other = (DataExample) o;
    if (!other.canEqual((Object)this)) return false;
    if (this.getName() == null ? other.getName() != null : !this.getName().equals(other.getName())) return false;
    if (this.getAge() != other.getAge()) return false;
    if (Double.compare(this.getScore(), other.getScore()) != 0) return false;
    if (!Arrays.deepEquals(this.getTags(), other.getTags())) return false;
    return true;
  }
  
  @Override public int hashCode() {
    final int PRIME = 59;
    int result = 1;
    final long temp1 = Double.doubleToLongBits(this.getScore());
    result = (result*PRIME) + (this.getName() == null ? 43 : this.getName().hashCode());
    result = (result*PRIME) + this.getAge();
    result = (result*PRIME) + (int)(temp1 ^ (temp1 >>> 32));
    result = (result*PRIME) + Arrays.deepHashCode(this.getTags());
    return result;
  }
  
}
```

- @Data [[어노테이션(Annotation)]]은 callSuper, includeFieldName, exclude와 같은 파라미터와 같이 사용될 수 없으므로, 해당 파라미터 사용이 필요할 때는 개별 [[어노테이션(Annotation)]]을 따로 다 명시해주면 되겠다.

```java
@Getter
@Setter
@ToString(includeFieldNames=true)
@EqualsAndHashCode
@RequiredArgsConstructor
public class Example {
    private String name;
    private double score;
}
```