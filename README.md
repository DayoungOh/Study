# Main Concepts

## 목차

6. 조건부 렌더링
7. 리스트와 Key
8. 폼
9. State
10. 합성 / 상속

## [조건부 렌더링]

 - if문이나 조건부 연산자(삼항연산자)를 사용해 현재 상태에 맞는 UI를 업데이트 한다.

### 로그인 여부 상태에 따라 로그인/로그아웃 버튼 바뀌도록 하는 Greeting 컴포넌트 생성하기
  - 예제
  ```
  function UserGreeting(props) {        
      return <h1>Welcome back!</h1>
  }
  function GuestGreeting(props) {
    return <h1>Please sign up.</h1>;
    }
  );

  function Greeting(props) {
      const isLoggedIn = props.isLoggedIn;
      if (isLoggedIn) {
          return <UserGreeting />;
      } return <GuestGreeting />;
  }

  ReactDOM.render(
    <Greeting isLoggedIn={false} />,
    document.getElementById('root')
    );

  ```

### 엘리먼트 변수
  - 엘리먼트를 저장하기 위해 변수를 사용하고, 출력의 다른 부분은 변하지 않은 채로 컴포넌트 일부를 조건부로 렌더링 할 수 있다.

  - 예제
    ```
    function LoginButton(props) {
        return (
            <button onClick={props.onClick}>
            Login
            </button>
        );
    }

    function LogoutButton(props) {
        return (
            <button onClick={props.onClick}>
            Logout
            </button>
        );
    }
    ```
  - 위의 두 컴포넌트를 사용해 현재 상태에 맞게 로그인/로그아웃 버튼을 렌더링하는 
    LoginControl 컴포넌트 만들기

    ```
    class LoginControl extends React.Component {
        constructor(props) {
            super(props);
            this.handleLoginClick = this.handleLoginClick.bind(this);
            this.handleLogoutClick = this.handleLogoutClick.bind(this);
            this.state = {
                isLoggedIn: false,
            }
        }

        handleLoginClick() {
            this.state({
                isLoggedIn: true,
            })
        }

        handleLogoutClick() {
            this.state({
                isLoggedIn: false,
            })
        }

        render() {
            const isLoggedIn: this.state.isLoggedIn;
            let button;
            if (isLoggedIn) {
                button = <LogoutButton onClick={this.handleLogoutClick} />;
            } else {
                button = <LoginButton onClick={this.handleLoginClick} />;
            }

            return (
                <div>
                    <Greeting isLoggedIn={isLoggedIn} />
                </div  >
            );
        }
    }

    ReactDOM.render(
        <LoginControl />,
        document.getElementById('root')
    );
    ```

### 논리 && 연산자로 if를 인라인으로 표현하기
  - `&& 뒤의 엘리먼트 조건이 true일 때 출력`되고, false라면 출력되지 않는다.
  - 예제
    ```
    function Mailbox(props){
        const unreadMeassages = props.unreadMessages;
        return (
            <div>
                <h1>Hello!</h1>
                {unreadMeassages.length > 0 &&
                    <h2>
                        You have {unreadMeassages.length} unread messages.
                    </h2>
                }
            </div>
        );
    }

    const messages = ['React', 'Re: React', 'Re:Re: React'];
        ReactDOM.render(
        <Mailbox unreadMessages={messages} />,
        document.getElementById('root')
    );
  ```

### 조건부 연산자로 IF-Else 구문 인라인으로 표현하기(삼항연산자)
  - 조건부 연산자인 `condition ? true : false`로 조건부 렌더링이 가능하다.
  - 예제
    ```
    render() {
        const isLoggedIn = this.state.isLoggedIn;
        return (
            <div>
                The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
            </div>
        );
    }
    ```

### 컴포넌트가 렌더링 하는걸 막기
  - 다른 컴포넌트에 의해 렌더링 되는 것을 막고 싶을 때, 렌더링 결과를 출력하는 대신 null을 반환하면 된다.
  - 예제
    ```
    function WarningBanner(props){
        if (!props.warn) {
            return null;
        }

        return (
            <div>
                Warning!
            </div>
        );
    }

    class Page extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
                showWarning: true
            };
            this.handleToggleClick = this.handleToggleClick.bind(this);
        }

        handleToggleClick() {
            this.setState(state => ({
                showWarning: !state.showWarning
            }));
        }

        render() {
            return(
                <div>
                    <WarningBanner warn={this.state.showWarning} />
                    <button onClick={this.handleToggleClick}>
                        {this.state.showWarning ? 'Hide' : 'Show'}
                    </button>
                </div>
            );
        }
    }

    ReactDOM.render(
        <Page />,
        document.getElementById('root')
    );

  ```

## [리스트와 Key]

### 자바스크립트의 map()
  - 자바스크립트에서 리스트를 변환할 때 사용하는 함수 map()
  - 예제
  ```
  const numbers = [1, 2, 3, 4, 5];
  const doubled = numbers.map((number) => number * 2);
  console.log(doubled);
  
  -> [2, 4, 6, 8, 10];
  ```

### React에서 map() 함수를 이용해 배열 결과 출력하기
  - 예제
  ```
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map(number) =>
      <li>{numbers}</li>
  );
  
  ReactDOM.render(
    <ul>{listItems}</ul>,
    document.getElementById('root')
  );
  ```

  - 일반적으로 컴포넌트 안에서 리스트를 렌더링한다.
  - 예제 
  ```
  function NumberList(props) {
      const numbers = props.number;
      const listItems = numbers.map((number) =>
          <li key={number.toString()}>
              {number}
          </li>
      );
      return (
          <ul>{listItems}</ul>
      );
  }
  
  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
      <NumberList numbers={numbers} />,
      document.getElementById('root')
  );
  ```

### Key
  - `key`: 엘리먼트 리스트를 만들 때 포함해야 하는 특수한 문자열 어트리뷰트
  - React가 어떤 항목을 변경, 추가 또는 삭제할지 식별하게 하는 역할
  - key는 엘리먼트에 안정적 고유성을 부여하기 위해 배열 내부의 엘리먼트에 지정해야 한다.
  - 리스트의 항목들 중 해당 항목을 고유하게 식별할 수 있는 문자열을 사용하며, 주로 데이터의 `id`를 key로 사용한다.
  - 예제
  ```
  const numbers = [1, 2, 3, 4, 5];
  const listItems = numbers.map((number) =>
      <li key={number.toString()}>
          {number}
      </li>
  );
  ```

  - 항목 순서가 바뀔 수 있는 경우 인덱스를 key로 사용하지 않는다.

  - key로 컴포넌트를 추출할 경우, 컴포넌트 안에 key를 부여하는게 아닌, 그 컴포넌트가 사용되는 엘리먼트가 key를 가져야 한다. 
  - `map() 함수 내부의 엘리먼트에 key를 넣어주는게 좋다.`
  ```
  function ListItem(props) {
      return <li>{props.value}</li>;
  }

  function NumberList(props) {
      const numbers = props.numbers;
      const listItems = numbers.map((number) =>
        <ListItem key={number.toString()} value={number} />
      );
      return (
          <ul>
            {listItems}
          </ul>
      );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  );
  ```

  - key는 형제 사이에서 고유한 값이어야 한다.
  ```
  function Blog(props) {
    const sidebar = (
        <ul>
          {props.posts.map((post) =>
              <li key={post.id}>
                  {post.title}
              </li>
          )}
        </ul>
    );

    const content = props.posts.map((post) =>
        <div key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.content}</p>
        </div>
    );

    return (
        <div>
            {sidebar}
        </div>
        <div>
            {content}
        </div>
    );
  }

  const posts = [
      {id: 1, title: 'Hello World', content: 'Welcome to learning React'},
      {id: 2, title: 'Installation', content: 'You can install React from npm.}
  ];
  ReactDOM.render(
      <Blog posts={posts} />,
      document.getElementById('root')
  );
  ```

## [Form]

### 제어 컴포넌트(Controlled Component)
  - 폼을 렌더링하는 리액트 컴포넌트는 폼에 발생하는 사용자 입력값을 제외하고 이렇게 제어되는 입력 폼 엘리먼트(input, textarea, select 등)를 제어 컴포넌트라고 한다.
  - 예제
  ```
  class NameForm extends React.Component {
      constructor(props) {
          super(props);
          this.state = {value: ''};

          this.handleChange = this.handleChange.bind(this);
          this.handleSubmit = this.handleSubmit.bind(this);
      }

      handleChange(e) {
          this.setState({
              value: e.target.value;
          })
      }

      handleSubmit(e) {
          alert('A name was sumitted: ' + this.state.value);
          e.preventDefault();
      }

      render() {
          return (
              <form onSubmit={this.handleSubmit}>
                <label>
                    Name:
                    <input type="text" value={this.state.value} onChange={this.handleChange} />
                </label>
                <input type="submit" value="Submit">
              </form>
          );
      }
  }
  ```

### textarea tag
  - React에서 textarea 엘리먼트는 텍스트를 자식으로 정의한다.
  - this.state.value를 생성자에서 초기화하므로 textarea는 일부 텍스트를 자식으로 가진 채로 시작된다는 점에 주의한다.
  - 예제
  ```
  class EssayForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
        value: 'Please write an essay about your favorite DOM element.'
        };

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value: event.target.value});
    }

    handleSubmit(event) {
        alert('An essay was submitted: ' + this.state.value);
        event.preventDefault();
    }

    render() {
        return (
        <form onSubmit={this.handleSubmit}>
            <label>
            Essay:
            <textarea value={this.state.value} onChange={this.handleChange} />
            </label>
            <input type="submit" value="Submit" />
        </form>
        );
    }
  }
  ```

### select tag
  - React에서는 최상단 select 태그에 value 어트리뷰트를 사용한다.
  - default로 설정된 옵션을 state의 value로 주는 점에 유의한다.
  - 예제
  ```
  class FlavorForm extends React.Component {
      constructor(props) {
          super(props);
          this.state = {value: 'coconut'};

          this.handleChange = this.handleChange.bind(this);
          this.handleSubmit = this.handleSubmit.bind(this);
      }

      handleChange(e) {
          this.setState({
              value: e.target.value
          });
      }

      handleSubmit(e) {
        alert('Your favorite flavor is: ' + this.state.value);
        e.preventDefault();
      }

      render() {
          return (
              <form onSubmit={this.handleSubmit}>
                <label>
                    Pick your favorite flavor:
                    <select value={this.state.value} onChange={this.handleChange}>
                        <option value="grapefruit">Grapefruit</option>
                        <option value="lime">Lime</option>
                        <option value="coconut">Coconut</option>
                        <option value="mango">Mango</option>
                    </select>
                </label>
                <input type="submit" value="Submit" />
              </form>
          );
      }
  }
  ```
  * select 태그에 multiple 옵션을 허용하면, value 어트리뷰트에 배열을 전달 할 수 있따
  ```
  <select multiple={true} value={['B', 'C']}>
  ```

### file tag
  ```
  <input type="file">
  ```




