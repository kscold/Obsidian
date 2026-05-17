- [[Jest]] 내부적으로 또하나의 [[Virtual DOM]]을 생성하여 [[테스트]]를 진행한다.

- 따라서 import "@testing-library/jest-dom"와 import { render, screen } from "@testing-library/react" 같이 임포트를 통한 render() 함수를 사용하여 [[JSX]] [[컴포넌트(Component)]]를 렌더링 한다.