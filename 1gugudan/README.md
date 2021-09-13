# React

# WHY?

1. 사용자 경험
2. 재사용 컴포넌트
3. 데이터 화면 일치

- 원시 리액트

    # 기본 틀

    ```html
    <!DOCTYPE html>
    <head>
        <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
        <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    </head>
    <body>
    <div id="root"></div>
    <script>
        const e = React.createElement;

        class LikeButton extends React.Component {
            constructor(props) {
                super(props);
            }

            render() {
                return e('button', null, 'Like'); //<button>Like</button>
            }
        }
    </script>
    <script>
        ReactDOM.render(e(LikeButton), document.querySelector('#root'));
    </script>
    </body>
    </html>
    ```

    1. react 불러오고, react-dom script 로드
    2. React가 LikeButton 컴포넌트를 만들겠다, javascript가 인지
    3. 다음 스크립트 → ReactDom.render → querySelector로 root Div 에다가 그린다고한다
    4. <div id="root"> 에 <button>Like</button> 그리기 [결과]
    - 미리 어떻게 그려질지 결과 예측해보기

    - 컴포넌트를 그릴곳이 필요(div id = root)
    - 버튼 필요

    ## 동작 Onlclick

    ```html
    render() {
                return e('button', null, 'Like'); //<button onclie="onclick">Like</button>
            }
    ```

    두번째 파라메터에 onclick 정의

    ```html
    render() {
                return e('button', {onClick: () => {console.log("clicked")} , type:'submit'}, 'Like'); 
                //<button onclick="() => {console.log('clicked') }" type="submit"> Like</button>
            }
    ```

    html 속성을 javascript로 표현할때는 camelcase로 작성 ( onclick→ onClick)

    ## State - 상태

    - 바뀔 수 있는 부분
    - 상태 정의 문법

        ```jsx
        class LikeButton extends React.Component {
                constructor(props) {
                    super(props);
                    this.state = {
                        liked : false,
                    };
                }
        ```

        버튼 동작을 render에 표현

        ```jsx
        render() {
                    return e('button', {onClick: () => { this.setState({liked : true})} , type:'submit'}, 'Like');
               
                }
        ```

        화면에 반영

        ```jsx
        render() {
                    return e(
                        'button', 
                        {onClick: () => { this.setState({liked : true})} , type:'submit'}, 
                        this.state.liked === true ? 'Liked' : 'Like',
                    );
                }
        ```

        상태를 데이터라고 생각했을때, 데이터와 화면(상태를)일치시켜준다

- 원시 리액트가 불편해서 나온 문법 / 바벨의 등장

    문법 개선

    ```jsx
    class LikeButton extends React.Component {
    constructor(props) {
                super(props);
                this.state = {
                    liked : false,
                };
            }

    render() {
                return <button type="submit"
                               onClick={() => {this.setState({liked : true})}}>
                    Liked
                </button>;
            }
    }
    ```

    ```jsx
    ReactDOM.render(<LikeButton />, document.querySelector('#root'));
    ```

    하지만 JavaScript에서는 html 태그를 이렇게 쓰는걸 지원을 안한다

    최신문법이랑 실험적인 문법들을 사용할 수 있게 해주는것이다.

    ```jsx
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    ```

    ```jsx
    <script type="text/babel">
        class LikeButton extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                    liked : false,
                };
            }

    render() {
                return <button type="submit" onClick={() => { this.setState({ liked : true })}}>
                    {this.state.liked === true ? 'Liked' : 'Like'}
                </button>;
            }
        }
    </script>
    <script type="text/babel">
        ReactDOM.render(<LikeButton />, document.querySelector('#root'));
    </script>
    ```

    이것이 JSX ( JS + XML )

    컴포넌트 여러개 사용

    ```jsx
    <script type="text/babel">
        ReactDOM.render(<div>
            <LikeButton />
            <LikeButton />
            <LikeButton />
            <LikeButton />
        </div>, document.querySelector('#root'));
    </script>
    ```

    JSX 문법( 실험적인 문법)을 지원하기 위해서 Babel 을 사용

    script type="text/babel" 로 하면 모든 브라우저에서 돌아가는 문법으로 알아서 변경해준다

    → 세부설정을 못하지만 해주려면 바벨 tool 이 따로 필요하다

    ```jsx
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    ```

    바벨 셋팅할 시간이없고 빠르게 바벨을 사용하려면 위처럼 사용하면되고,

    최신 문법 최신 객체는 babel polyfill 을 하나 더 추가해줘야 한다.

바뀌는것들 State

안바뀌는것들은 Tag

- 중괄호를 쓰면 javascript를 사용할 수 있다.

    ```jsx

    render(){
                return (
                    <div>
                        <div>{this.state.firstNumber} 곱하기 {this.state.secondNumber} 는?</div>
                    </div>
                );
    ```

- 구구단 게임 만들기

    ```html
    <!DOCTYPE html>
    <head>
        <meta charset="UTF-8">
        <title>구구단</title>
        <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
        <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
        <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    </head>
    <body>
    <div id="root"></div>
    <script type="text/babel">
        class GuGuDan extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                  firstNumber : Math.ceil(Math.random() * 9),
                  secondNumber : Math.ceil(Math.random() * 9),
                  inputValue : '',
                  result : '',
                };
            }

            render(){
                return (
                    <div>
                        <div>{this.state.firstNumber} 곱하기 {this.state.secondNumber} 는?</div>
                        <form>
                            <input type="number" value={this.state.inputValue} />
                                <button>입력!</button>
                        </form>
                        <div>{this.state.result}</div>
                    </div>
                );
            }
        }
    </script>
    <script type="text/babel">
        ReactDOM.render(<GuGuDan />, document.querySelector('#root'));
    </script>
    </body>
    </html>
    ```

    - Javascript 안에서 HTML tag 를 사용할때, 싱글태그도 닫아줘야한다( input)
    - HTML 태그안에서 Javascript Value를 사용하려면 { } 로 사용해준다.

    ```jsx

    <input type="number" value={this.state.inputValue} onChange={() => this.setState({inputValue: e.target.value})} />
    	<button>입력!</button>

    ```

    setState로 현재 입력한 e.target.value 를 state로 입력시켜준다

    ```jsx
    input.onchange = (e) => {console.log(e.target.value)}
    입력하고있는걸 console에 적는 script
    ```

    setState 는 직접 바꾸는 부분들을 State로 만들어 바꾼다

    form을 submit 했을때 상황 코딩

    ```jsx
    <form onSubmit={(e) => {
                            e.preventDefault();
                            if (parseInt(value) === this.state.firstNumber * secondNumber){
                                this.setState({
                                    result:'정답',
                                    firstNumber: Math.ceil(Math.random() * 9),
                                    SecondNumber: Math.ceil(Math.random() * 9),
                                    inputValue: '',
                                })
                            }
                            else{
                                this.setState({
                                    result: '땡',
                                    inputValue: '',
                                });
                            }
                        }}>
    ```

    클래스와 메소드 분리

    ```jsx
    onSubmit = (e) =>{
                e.preventDefault();
                if (parseInt(value) === this.state.firstNumber * this.state.secondNumber){
                    this.setState({
                        result:'정답',
                        firstNumber: Math.ceil(Math.random() * 9),
                        SecondNumber: Math.ceil(Math.random() * 9),
                        inputValue: '',
                    })
                }
                else{
                    this.setState({
                        result: '땡',
                        inputValue: '',
                    });
                }
            }

            onChange = (e) =>{(e) => this.setState({inputValue: e.target.value})};

            render(){
                return (
                    <div>
                        <div>{this.state.firstNumber} 곱하기 {this.state.secondNumber} 는?</div>
                        <form onSubmit="" {this.onSubmit}>
                            <input type="number" value={this.state.inputValue} onChange={this.onChange} />
                                <button>입력!</button>
                        </form>
                        <div>{this.state.result}</div>
                    </div>
                );
            }
    ```

    1. constructor →변할것들 을 state로 만들어준다
    2. render → 변할것들 위치에 state 위치에 잘 넣어준다.
    3. 변하는부분을 컨트롤하는 함수 작성 및 이벤트 리스너 작성

    - render 에서 Component를 <div> 로 감싸줘야했는데
    - <> </> 필요없는 div를 이렇게 빈 태그로 감싸줘도된다.
        - 만약 바벨 2 설치가 안되어있어서 에러가 난다면 <React.Fragment> </React.Fragment>로 감싸주면 된다
    -