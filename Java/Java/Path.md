- Path는 [[인터페이스(Interface)]], Paths는 [[클래스(Class)]]으로 자바 7에서 추가된 java.nio.file 패키지에 속한 [[인터페이스(Interface)]]이다.
- [[파일 업로드]]에 주로 사용된다.

- Path는 OS에 독립적으로 파일, 디렉토리의 경로를 표현하고 다룰 수 있는 방법을 제공한다.

- Path를 통해, 파일, 디렉토리 옮기기, 복사, 삭제, 읽기, 쓰기 등의 작업을 수행할 수 있다.  
- 뿐만 아니라, 파일 시스템과 소통할 수 있는 방법을 제공하므로, 디렉토리 생성, 파일 존재 여부 확인, 정보 불러오기 등의 작업을 수행할 수 있다.

- Paths [[클래스(Class)]]는 Path를 다룰 수 있는 여러 [[메서드(Method)]]를 제공한다.  
- Paths 또한 java.nio.file 패키지에 속해있다.

- 중요한 점은, Path는 단순히 경로정보를 캡슐화한 것이다. 
- 파일의 존재 여부는 상관 없다.

## Path, Paths 메서드

### Paths.get()

- 첫 번째 매개변수는 디렉토리 경로이다.
- 두 번째 매개변수는 파일 또는 디렉토리의 이름을 설정할 수 있다.
- 두 매개변수를 결합하여 전체 파일 또는 디렉토리의 경로를 얻을 수 있다.


```java
Path filePath1 = Paths.get("example.txt"); // file
Path filePath2 = Paths.get("example_dir/example.txt"); // file
Path dirPath= Paths.get("example_dir"); // directory
Path dirPath2= Paths.get("./parent/child"); // directory
Path noPath = Paths.get("not_existing_file_name.txt"); // 파일의 존재 여부와는 상관없음

String fileName = filePath2.getFileName(); // example.text
String fileName2 = dirPath2.getFileName(); // child

File file = filePath1.toFile(); // 경로에 위치한 파일을 반환
```

## 절대 경로, 상대 경로

- Paths를 통해 절대 경로, 상대 경로 두 방법으로 쉽게 파일에 접근할 수 있다.

```java
// 상대 경로
Path relativeFilePath = Paths.get("relative/path/to/file.txt");
// src/main/java/.../relative/path/to/file.txt

// 절대 경로
Path absoluteFilePath = Paths.get("/root-directory/path/to/file.txt");
// /root-directory/path/to/file.txt
```

- 절대 경로를 사용할 때, 경로의 시작점(기준점)은 파일 시스템의 root directory이다.  
- 다른 말로, 절대 경로는 항상 파일 시스템 계층의 최상위 레벨을 표현하기 위해 forward slash `/`로 시작한다.

- UNIX기반의 시스템에서, root directory는 single forward slash `/`로 표현되고, 모든 다른 디렉토리들과 파일들은 file system hierarchy 아래에 위치한다.  

- 예를 들어, 절대 경로 `"/home/user/documents/file.txt"`은 root directory에서 시작해서, `"file.txt"` 파일에 도달하기 전에 `"home"`, `"user"`, `"documents"` 디렉토리를 거친다.

- 상대경로는 프로젝트 루트에서 시작한다.

## File, Files [[클래스(Class)]]

- `File`은 `java.io`에, `Files`는 `java.nio`에 속한 [[클래스(Class)]]이다.

- `File`은 파일 시스템에서의 파일과 디렉토리에 대한 representation을 제공한다.  
- `Files`는 파일 시스템을 통해, 파일을 다루는 여러 유틸리티 [[메서드(Method)]]를 제공한다.

- `Files`는 `File`과 달리, 주로 `Path`[[객체(Object)]]를 사용해 [[메서드(Method)]]를 사용한다.

## File, Files 예시

- 파일 읽어오는 방식이다.

```java
File file = new File("test.txt");
// 존재하므로 true
file.canRead(); 

File noFile = new File("no.txt");
// 존재하지 않으므로 false
noFile.canRead(); 

```

- 파일 생성하는 방식이다.

```java
try {
	noFile.createNewFile();
    Files.createNewFile(Paths.get("test.txt"));
} catch (IOException e) {}
```

- 파일명을 변경하는 방식이다.

```java
File from = new File("from.txt");
File to = Files.get(Paths.get("to.txt"));
// from.txt -> to.txt
from.renameTo(to);

// Files로 파일명 변경하기
Path oldPath = Paths.get("from.txt");
Path newPath = Paths.get("to.txt");
Files.move(oldPath, newPath, StandardCopyOption.REPLACE_EXISTING);
```

- 파일에 데이터를 쓰는 방식이다.

```java
File file = new File("test.txt");
FileOutputStream fos = new FileOutputStream(file);
fos.write("hello worlds".getBytes());
fos.close(); // or fos.flush()

Files.write(Paths.get("test.txt"), "hello worlds".getBytes());
```

- 파일을 읽는 방식이다.

```java
File file = new File("test.txt");
FileInputStream fis = new FileInputStream(file);
byte[] data = new byte[(int) file.length()];
fis.read(data);
fis.close();
String text = new String(data, "UTF-8");

byte[] data = Files.readAllBytes(Paths.get("test.txt"));
String text = new String(data, "UTF-8");
```

- 밑의 예시는 여러 방법으로 파일 다루는 코드이다.

```java
String rawPath = "src/main/resources/templates"; // 파일 패스 문자열 정의
Path path = Paths.get(rawPath); // Paths 클래스 메서드를 사용하여 

try {
	Stream<Path> list = Files.list(path); // 특정 경로에 위치한 파일들
    list.forEach(p -> { // forEach를 통해 요소마다 메서드를 실행
		System.out.println(Files.isDirectory(p)); // 디렉토리인지 판별
		System.out.println(Files.isRegularFile(p)); // 파일인지 판별
    })
} catch (IOException e) {}
```