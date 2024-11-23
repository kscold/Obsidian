- [[리액트(React)]]에서  Drag and Drop을 사용할 수 있는 라이브러리이다.

### 구조

#### DragDropContext

DragDropContext은 Drag and Drop이 일어나는 전체영역이다.  
Droppable, Dragpable 이 지정된 영역을 포함 하고 있어야하며,  
동작의 핵심이 되는  
`onDragEnd={...}`,`onDragStart={...}`  
두 가지의 함수를 바인딩 하기 때문에 반드시 설정해줘야 하는 부분이다.

예시)

```javascript
import { DragDropContext} from "react-beautiful-dnd";

	<DragDropContext
		onDragEnd={props.handleDragEnd}
		onDragStart={props.handleDragStart}
	>
    
	{props.categoriesData?.fetchProcessCategories.map(
		(el: any, index: any) => (
            <Droppable
                droppableId={String(el.processCategoryId)}
                key={index}
            >
                ...
            </Droppable>
		)
	}
    <AddColumnBtn
    	projectId={props.projectData?.fetchProject.projectId}
    />
    /DragDropContext>
```

#### Droppable

Droppable은 Drop이 일어나는(가능한) 영역이다.  
이 영역에서 Item을 Drop할 경우에 `DragDropContext` 바인딩 된 `onDragEnd={...}`  
함수가 동작하며 최종적인 Drag and Drop 동작에 대한 dom을 그려주기에  
필수적으로 영역을 지정해줘야한다.

Droppable는 `droppableId` 를 필수적으로 입력해줘야하,  
드롭될 영역의 고유 ID를 뜻한다.  
아래 예시에서는 `Array.map()` 메서드를 사용하여 각가의 고유한 영역을 지정하였고,  
각각의 영역이 받는 처음 값을 `droppableId`로 지정해놨다.(uuid를 활용해도 좋다.)  
이 값은 string 형태의 값으로 변환해서 넣어주어야 한다.

**provided**는 `provided.innerRef`를 참조하여 동작을 실행하는 매개변수 이기에  
반드시 들어가야하는 사항이며

**snapshot**은 동작시 dom 이벤트에 대하여 적용될 style 참조를 뜻한다.  
선택적항목이다.

예시)

```javascript
import { Droppable } from "react-beautiful-dnd";

<DragDropContext
        onDragEnd={props.handleDragEnd}
        onDragStart={props.handleDragStart}
      >
        {props.categoryName?.map((el: string[], index: number) => (
          <Droppable droppableId={el[0]} key={index}> 
            {(provided, snapshot) => (
              <div ref={provided.innerRef} {...provided.droppableProps}>
                <ExperienceSMAFDetail
                  key={index}
                  el={el}
                  index={index}
                  scheduleArray={props.scheduleArray}
                />
                {provided.placeholder}
              </div>
            )}
          </Droppable>
        ))}
        <S.AddcolumnBtn>
          항목추가
          <S.AddCoulumnIcon
            onClick={props.AddColumn}
            src="/detailPage/addcolumn.png"
          ></S.AddCoulumnIcon>
        </S.AddcolumnBtn>
      </DragDropContext>
```

#### Draggable

Dragpable은 Drag가 일어나는 요소들으로 Droppable 영역안에서 움직을 요소들을 정해준다.  
Drag이벤트가 시작되면 `DragDropContext` 바인딩 된 `onDragStart={...}`가 동작한다.

**draggableId**를 필수적으로 받아와야하는데 Droppable 영역에서는 DND가 일어날 영역의 고유값을 입력해 주었다면 Draggable에서는 움직일요소들이 가지는 고유값을 입력해 주어야한다.

Dragpable 역시 provided와 snapshot를 제공하며  
**provided**는 `provided.innerRef`를 참조하여 동작을 실행하는 매개변수 이기에  
반드시 들어가야하는 사항이며

**snapshot**은 동작시 dom 이벤트에 대하여 적용될 style 참조를 뜻한다.  
선택적항목이다.