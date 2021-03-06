# React Components와 Elements, Instances
## Elements
- 리액트 요소는 컴퍼넌트 인스턴스 혹은 DOM 노드와 그들의 속성을 설명하는 일반 객체이다.
  - 자바스크립트 객체는 가볍기 때문에 오버헤드 없이 요소들을 만들고 없앨 수 있다.
  - React가 객체를 분석하고, 이전의 객체 표현과 비교하여 변경 사항 확인 후, 변경이 일어난 경우에만 실제 DOM을 업데이트할 수 있다.
  - DOM node를 만들기 위해 `React.createElement`메소드를 사용할 수 있다.
- 요소는 실제 인스턴스가 아니며, 화면에서 보고싶은 것을 알려주는 일종의 방법이라고 할 수 있다.
- 요소에 대한 어떤 메소드도 호출할 수 없다.
- 요소는 `type : (string | ReactClass)`과 `props : Object`의 두 필드가 존재하는 변경불가능한 객체이다.

### DOM Elements
```js
const helloElement = React.createElement(
  'div',
  { id: 'hello-world' },
  'Hello, World!'
);
```

- `createElement`는 3개의 인자를 받으며 각각의 인자는 다음과 같다.
  - 태그 이름
  - 요소의 속성
  - 요소의 내용 또는 하위 요소
- `createElement`는 아래와 모양이 비슷한 객체를 반환한다.
- 요소 tree를 만드려면, 아래와 같이 하나 이상의 자식 요소를 `children` 속성으로 지정한다.

```js
{
  type: 'div',
  props: {
    children: 'Hello, World!',
    id: 'hell-world'
  }
}
```

- 그 다음, DOM에 렌더링될 때, 다음과 같은 DOM 노드가 렌더링된다.

```html
<div ='hell-world'>Hello, World!</div>
```

### Component Elements
- 위에서 사용된 요소의 `type`속성은 리액트 컴퍼넌트에 해당하는 함수 혹은 클래스가 될 수도 있다.
- 컴퍼넌트를 묘사하는 요소 또한 DOM 노드를 묘사하는 요소처럼 요소일 뿐이다.
- 요소는 서로 중첩되어 혼합될 수 있다.

```js
const DangerButton = ({ children }) => ({
  type: Button,
  props: {
    color: 'red',
    children: children
  }
});

const DeleteAccount = () => ({
  type: 'div',
  props: {
    children: [{
      type: 'p',
      props: {
        children: 'Are you sure?'
      }
    }, {
      type: DangerButton,
      props: {
        children: 'Yep'
      }
    }, {
      type: Button,
      props: {
        color: 'blue',
        children: 'Cancel'
      }
   }]
});
```
```jsx
/* with JSX */
const DeleteAccount = () => (
  <div>
    <p>Are you sure?</p>
    <DangerButton>Yep</DangerButton>
    <Button color='blue'>Cancel</Button>
  </div>
);
```

### Element 트리를 캡슐화하는 Components
```js
{
  type: Button,
  props: {
    color: 'blue',
    children: 'OK!'
  }
}

// It would be like this
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
```
- 리액트는 페이지의 모든 컴퍼넌트들에 대한 기본 DOM 태그 요소를 알 때까지 트리 순회를 한다.
- 리액트 컴퍼넌트에게 있어 `props`는 입력이며, `요소 트리`는 출력이다.
  - `props`는 한 방향으로 흐른다.
- 이러한 구성으로 인해, 내부 DOM에 의존하지 않고 UI의 독립적인 부분을 작성할 수 있다.

## Components
- 컴퍼넌트는 (선택적으로) 입력을 받아들이고, React Element를 반환하는 함수 혹은 클래스이다.
- 클래스로 정의된 컴퍼넌트는 함수로 정의된 컴퍼넌트보다 강력하다.
  - 지역 `state`를 저장할 수 있고, 생명주기 메소드 등을 활용하여 DOM 노드가 생성되거나 삭제될 때 등의 생명주기마다 사용자 정의의 로직을 실행할 수 있다.
  - 클래스로 정의된 컴퍼넌트만이 인스턴스를 가진다.
    - 리액트는 모든 클래스 컴퍼넌트에 대한 인스턴스 생성을 담당한다.
    - 메소드 및 지역 `state`를 통해 객체 지향적인 방법으로 컴퍼넌트를 작성할 수 있다.
  - 부모 컴퍼넌트 인스턴스가 하위 컴퍼넌트 인스턴스에 접근하는 방법이 존재하지만, 일반적으로는 이 방법을 피해야 하며, 필수적인 작업에만 사용한다.
    - ex) 필드에 포커스하는 작업 등
- 함수로 정의된 컴퍼넌트는 단순하다.
  - `render` 메소드만 있는 클래스 기반 컴퍼넌트와 동일한 역할을 한다.
  - 클래스에서만 사용할 수 있는 기능이 필요하지 않은 경우, 함수 컴퍼넌트를 사용하는 것이 좋다.
- 함수, 클래스 상관없이 기본적으로 그들은 모두 리액트에 대한 컴퍼넌트다.
  - 위에서 언급한 것처럼, `props`를 그들의 입력으로 사용하며, 요소를 출력으로 반환한다.

## Instances
- 인스턴스는 클래스 컴퍼넌트에서 `this`를 참조하며, 이는 지역 `state`를 저장하고, 생명주기 메소드를 사용하는 데 유용하다.
- 클래스 컴퍼넌트는 인스턴스가 있다.
  - 하지만, 리액트가 이를 처리하기 때문에 컴퍼넌트 인스턴스를 직접 만들 필요는 없다.
- 함수 컴퍼넌트는 인스턴스가 전혀 없다.


## Reference
- https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html
