- 브라우저가 [[서버(Server)]]에서 페이지에 대한 [[HTML(Hyper Text Markup Language)]] 응답을 받고 화면에 표시하기 전에 단계을 웹 페이지 빌드 과정(CRP)라고 한다.

- [[웹(Web)]] 브라우저가 [[HTML(Hyper Text Markup Language)]] 문서를 일고, 스타일을 입히고 뷰포트에 표시하는 과정이다.


![[Pasted image 20240626160831.png]]


## CRP의 과정

### 1. DOM tree 생성

- Render engine이 문서를 읽어들여서 그것들을 [[파싱(Parsing)]]하고 어떤 내용을 페이지에 렌더링할지 결정한다.

### 2. Render tree 생성

- 이 단계에서 [[DOM(Document Object Model)]]과 [[CSSOM(CSS Object Model)]]이 결합하는 곳이며 이 프로세스는 화면에 보이는 모든 컨텐츠와 스타일 정보를 모두 포함하는 최종 렌더링 트리를 출력한다.
- 즉 화면에 표시되는 모든 노드의 컨텐츠 및 스타일 정보를 포함한다.

### 3. Layout

- 브라우저가 페이지에 표시될 각 요소의 크기와 위치를 계산 단계이다.

### 4. Paint

- 실제 화면에 출력(그리는)하는 단계이다.


## 바닐라 [[CRP(Critical Rendering Path)]]의 문제점

- 어떤 인터렉션에 의해 [[DOM(Document Object Model)]]에 변화가 발생하면 그 때 마다 Render Tree가 재생성 된다.
- 즉, 모든 요소들의 스타일을 다시 계산하고 Layout과 Repaint 과정까지 다시 거치게 된다.

- 따라서 인터렉션이 적은 [[웹(Web)]]이라면 괜찮지만 만약 인터렉션이 엄청나다면 작은 변화로 인해 위에 필요한 과정을 계속 거치게 되니 불필요하게 [[DOM(Document Object Model)]]을 조작하는 비용이 너무 크게 된다.

- 따라서 이런 문제를 해결하기 위해 나오게 된 것이 [[Virtual DOM]]이다.