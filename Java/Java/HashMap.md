- HashMap은 [[Map]] [[인터페이스(Interface)]]를 구현한 대표적인 Map 컬렉션([[Collection]])이다.
- Map 인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있다.
- Map은 키와 값으로 구성된 Entry [[객체(Object)]]를 저장하는 구조를 가지고 있는 자료구조이다.

- 여기서 키(key)와 값(value)은 모두 객체이다. 
- 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없다.
- 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치된다.

- HashMap은 이름 그대로 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 가진다.

## HashMap [[메서드(Method)]]
### map.entrySet()

```java
public Set<Map.Entry<K,​V>> entrySet()
```

- map.entrySet() 메서드 당 map의 key와 value를 가지는 [[Set]] [[객체(Object)]]를 반환한다.


```java
import java.util.HashMap;
import java.util.Map;
import java.util.Map.Entry; 

public class HashMapPrint {    
	
	public static void main(String[] args) {         // HashMap 준비   
		Map<Integer, String> map = new HashMap<Integer, String>();       
		
		map.put(1, "Apple");        
		map.put(2, "Banana");        
		map.put(3, "Orange"); 
		
		// for loop (entrySet())        
		for (Entry<Integer, String> entrySet : map.entrySet()) {            
			System.out.println(entrySet.getKey() + " : " + entrySet.getValue());        
		}     
	}
}

// >> 1 : Apple2 : Banana3 : Orange
```


- map.entrySet() [[메서드(Method)]]를 호출하여 map의 key와 value를 포함하는 Entry객체의 Set을 얻어온다.
- 그리고, 이 Set 객체를 순회하면서map의 key와 value를 출력한다.

## map.keySet(), mep.get()

```java
public Set<K> keySet()
```

- map의 key들을 모아서 Set 형태로 반환한다.

```java
public V get​(Object key)
```

- 매개변수로 key값을 전달하면, map에서 해당 key의 value를 찾아서 반환한다.


```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set; 

public class HashMapPrint {
	
	public static void main(String[] args) {         
		// HashMap 준비        
		Map<Integer, String> map = new HashMap<Integer, String>();        
		
		map.put(1, "Apple");        
		map.put(2, "Banana");        
		map.put(3, "Orange");         
		
		// for loop (keySet())        
		Set<Integer> keySet = map.keySet();        
		for (Integer key : keySet) {            
			System.out.println(key + " : " + map.get(key));        
		}    
	}
}

// >> 1 : Apple2 : Banana3 : Orange
```

```java
Set<Integer> keySet = map.keySet();
```

- 먼저 map.keySet() [[메서드(Method)]]를 호출하여, key 목록을 Set 형태로 가지고 온다.

```java
for (Integer key : keySet) {
	System.out.println(key + " : " + map.get(key));
}
```

- key Set을 순회하면서 key를 출력하고, 해당 key를 가지고, map.get() 메서드를 호출하여 해당 key의 value를 출력하였다.

### map.values() 

- value만 가져올 때 사용한다.

```java
public Collection<V> values()
```

- HashMap의 values()는, 해당 map의 value 목록을 [[Collection]] 형태로 리턴한다.

```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Map; 

public class HashMapPrint {   
	public static void main(String[] args) { // HashMap 준비        
		Map<Integer, String> map = new HashMap<Integer, String>();        
		
		map.put(1, "Apple");        
		map.put(2, "Banana");        
		map.put(3, "Orange"); // map.values()        
		
		Collection<String> values = map.values();       
		System.out.println(values);  // [Apple, Banana, Orange]    
	}
}

// >> [Apple, Banana, Orange]
```


```java
Collection<String> values = map.values();
```

- map.values()에서 가져온 value 목록을 출력한다.


## 반복하여 출력하는 방법
### [[Iterator()]] 사용


```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Map.Entry; 

public class HashMapPrint {    
	public static void main(String[] args) {  
		// HashMap 준비       
		Map<Integer, String> map = new HashMap<Integer, String>();        
		
		map.put(1, "Apple");        
		map.put(2, "Banana");        
		map.put(3, "Orange");         
		
		// Iterator        
		Iterator<Entry<Integer,String>> it = map.entrySet().iterator(); 
		// key와 value를 모두 반환하는데 iterator를 이용하여 반복        
		
		while(it.hasNext()) {            
			Entry<Integer, String> entrySet = (Entry<Integer, String>) 
			it.next();            
			
			// key, value 출력            
			System.out.println(entrySet.getKey() + " : " + entrySet.getValue());       
		}
	}
}

// >> 1 : Apple2 : Banana3 : Orange
```

- map.entrySet() 이 리턴하는 Set 객체의 Iterator를 사용하여 key, value 목록을 출력하였다.

## forEach 사용

  - Java 8 버전 이후부터 [[forEach()]]를 [[람다(lambda)]]식을 사용하여 반복할 수 있다.

```java
import java.util.HashMap;
import java.util.Map; 

public class HashMapPrint {    
	public static void main(String[] args) { 
		// HashMap 준비        
		
		Map<Integer, String> map = new HashMap<Integer, String>();        
		
		map.put(1, "Apple");        
		map.put(2, "Banana");        
		map.put(3, "Orange");         
		
		// forEach        
		map.forEach((key, value) -> {            
			System.out.println(key + " : " + value);        
		});    
	}
}

// >> 1 : Apple2 : Banana3 : Orange 
```

- Java 8 이후로는 forEach 문을 사용하여 map을 순회하면서 key와  value를 출력할 수 있다.
