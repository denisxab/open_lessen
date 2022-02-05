# React

## Подготовка

### Установка

Нужно иметь:

- `nodejs` >= 14.0
- `npm` >= 6.0
- Для отладки `React`приложения есть расширение для браузера `React Developer Tools` (Он работает только в боевом режиме, в режиме разработчика он не работает)

Установка:

- Linux
    - `Ubuntu`
        ```bash
        sudo apt install nodejs
        ```
    - `Arch`
        ```bash
        sudo pacman -S  nodejs
        ```

### Подключить `TypeScript`

1. Инициализировать `npm`

    ```bash
    npm init
    ```

2. Установить зависимости

    ```bash
    npm install --save typescript @types/node @types/react @types/react-dom @types/jest
    ```

3. Глобально скачать шаблон для `React`

    ```bash
    sudo npm install -g create-react-app
    ```

4. Создадим проект
    ```bash
    npx create-react-app <ИмяПроекта> --template typescript
    ```

- Все зависимости
    ```json
    {
    	"name": "...",
    	"version": "1.0.0",
    	"description": "...",
    	"main": "index.tsx",
    	"scripts": {
    		"dev": "webpack-dev-server --config webpack.config.js",
    		"start": "react-scripts start",
    		"build": "react-scripts build",
    		"test": "react-scripts test",
    		"eject": "react-scripts eject"
    	},
    	"author": "...",
    	"license": "ISC",
    	"devDependencies": {
    		"@types/react": "^16.8.24",
    		"@types/react-dom": "^16.0.5",
    		"@types/webpack": "4.1.4",
    		"css-loader": "^6.5.1",
    		"style-loader": "^3.3.1",
    		"ts-loader": "^6.2.1",
    		"typescript": "^3.4.3",
    		"webpack": "^5.65.0",
    		"webpack-cli": "^4.9.1",
    		"webpack-dev-server": "^4.6.0"
    	},
    	"dependencies": {
    		"dotenv": "^10.0.0",
    		"react": "^16.12.0",
    		"react-dom": "^16.12.0"
    	}
    }
    ```

### React проект

Собрать и запустить сервер с `React` [ +](https://create-react-app.dev/docs/deployment/)

```js
npm install -g serve
serve -s build
```

---

Структура `React`проекта

- `build`

- `node_modules` = Хранится скаченные библиотеки

- `public`

    - `index.html` = Главный `html` файл. (в него мы можем подключать необходимые библиотеки и стили)

        ```html
        <!DOCTYPE html>
        <html lang="en">
        	<head>
        		<meta charset="utf-8" />
        		<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
        		<meta
        			name="viewport"
        			content="width=device-width, initial-scale=1"
        		/>
        		<title>React App</title>
        	</head>
        	<body>
        		<noscript
        			>You need to enable JavaScript to run this app.</noscript
        		>
        		<div id="root">
        			<!-- Здусь будет наше `JS`приложение -->
        		</div>
        	</body>
        </html>
        ```

- `src`

    - `App.js` = Файл для `React`кода
    - `index.js`= Главный `js` файл. (Глобальные скрипты проекта)
    - `index.css` = Главный `css` файл. (Глобальные стили проекта)

- `package.json` = Конфигурации `React`проекта (для `npm`)

- `package-lock.json` = Конфигурации `React`проекта (для `yarm`)(Можно удалить если используете `npm`)

---

## Синтаксис `jsx`

`JSX` - это расширение синтаксиса языка JavaScript, которое обеспечивает способ структурирования рендеринга компонентов с использованием синтаксиса, знакомого многим разработчикам. По внешнему виду он похож на `HTML`.

---

| HTML    | JSX         | Описание   |
| ------- | ----------- | ---------- |
| `class` | `className` | Класс тега |
|         |             |            |

Обращатся к JS можно просто использую фигурные скобки`{}`

```jsx
{
	ИмяПеременной;
}
```

---

### Использовать `CSS` стили в `JSX`:

- Если стиль имеет `-` то нужно заменить следующую букву на заглавную `border-radius` = `borderRadius`

    ```js
    import './App.css';
    import React, {useState} from "react";

    function App() {

    	// Переменная в которой хранятся `CSS` стили
    	const style = {
    		<ЛюбоеНазвание>: {
    			// CSS стиль
    			// Ключ: значения,
    			// background: '#af4949', color: '#000',
    		}
    	}
    	return (
    		<div className='box_main'>

    			<div style={style.<ЛюбоеНазвание>} ></div>

    		</div>
    	);
    }

    export default App;
    ```

- Использование `CSS` файлов в `JSX`. Для этого файл `CSS` должен иметь расширение `<ИмяФайла>.module.css`

    ```css
    .<ИмяКласса>{
    	/*CSS стиль*/
    	/*background: #000;*/
    }
    ```

    Нужно импортировать объект `classes`, и через него обращаться к классам `CSS`

    ```js
    import './App.css';
    import React, {useState} from "react";

    import classes from './<ИмяФайла>.module.css'; //!!!


    function App() {

    	return (
    		<div className={classes.<ИмяКласса>}>



    		</div>
    	);
    }

    export default App;
    ```

---

```ts
import React from "react";
import './index.css';

const App: React.FunctionComponent = () => {
    return (
        <div className="mt-3">
            <h1 className={'h1_class'}> Hello React + Django + Python</h1>

            <div className="d-flex flex-row justify-content-around mt-4" style={{height: "200px",background: "#000"}}>

                <div className="p-5 " style={{background: "#535353"}}>
                    Flex 1
                </div>
                <div className="p-5 " style={{background: "#535353"}}>
                    Flex 2
                </div>
                <div className="p-5 " style={{background: "#535353"}}>
                    Flex 3
                </div>
            </div>
        </div>
    );
}

export default App;
```

## Компоненты

### Импорт модулей

Импортируем `jquery`. `jquery` в `React` не требует использование `$(document).ready()`

```js
import $ from "jquery";
```

> Скачать `jquery`
>
> ```bash
> npm install  jquery
> ```

### Импорты компонентов

Структура создания компонентов

- `ИмяReactПроекта`
- `src`
    - `ЛюбоеИмяПапки`
        - `ЛюбоеИмяКомпонента.js`

---

Основные правила `App.js`:

1. Весь итоговый `jsx` текст мы пишем в `function App()`

```js
import './App.css';


function App() {


	return (


		// JSX Код

	);
}

export default App;
```

---

Основные правила `Компонент.js`:

1. Все теги должны находится в одном главном теги `div`.
2. Нужно импортировать `import React from "react";`
3. Нужно возвращать функцию с `return ( //JSX код )` из компонента
4. Для того чтобы вставить `JSX` код из функции другого компонента нужно вызвать её в виде `<ИмяФункцииКомпанента/>`
5. У каждого компонента своя область локальная видимости. То есть у двух одинаковых компонентов подключенных к одному другому компоненту(главному) не будет конфликта имен.
6. В компонент можно передавать аргументы, в итоги эти аргументы соберутся в объект, и этот объект обычно называют `props`. Мы можем сразу раскрыть объект использую синтаксис `{props_1, props_2, props_... }`

```tsx
import React from "react";

interface ИмяИнтерфейса{
	Ключ_...:  ТипКлюча
}

function ИмяФункцииКомпанента: React.FunctionComponent<ИмяИнтерфейса>(props) {

	console.log(props)

	return (
		// JSX Код

		{props.<Ключ_...>}
	)
}

// Экспортируем функцию наружу(чтобы её можно было импортировать)
export default Basket
```

Можно сразу раскрывать аргумент `props` используя синтаксис `{props_1, props_2, props_... }`

```tsx
import React from "react";

interface ИмяИнтерфейса{
	Ключ_...:  ТипКлюча
}


function ИмяФункцииКомпанента:React.FunctionComponent<ИмяИнтерфейса>( { Ключ_..., }) {

	console.log(props)

	return (
		// JSX Код

		{Ключ_...}
	)
}

// Экспортируем функцию наружу(чтобы её можно было импортировать)
export default Basket
```

---

`App.js`. Для того чтобы вставить `JSX` код из функции другого компонента нужно вызвать её в виде `<ИмяФункцииКомпанента/>`

```js
import './App.css';
import <ИмяФункцииКомпанента> from "./<ЛюбоеИмяКомпонента>/<ИмяФункцииКомпанента>";

function App() {

	return (
		<div className='ИмяКласса'>

		<ИмяФункцииКомпанента Аргументы_1_В_Компанет={"Значение_1"}  Аргументы_2_В_Компанет={"Значение_2"}   />

		</div>

);}

// Экспортируем функцию наружу(чтобы её можно было импортировать)
export default App;

```

---

## React Хуки

- [Докуменатция по все хукам](https://ru.reactjs.org/docs/hooks-reference.html#useref)

### `useState` - Переменная состояния

```js
// js
const [<ИмяПеременной>, <ФункцияДляИзмененияПеременной>] = useState(<ЗначениеПоУмолчанию>)
// ts
const [<ИмяПеременной>, <ФункцияДляИзмененияПеременной>] = useState<ТипЗначений>(<ЗначениеПоУмолчанию>)
```

Для того чтобы изменять данные в `html` нужно использовать состояние. Потому что `React` использует виртуальный `DOM`, и для того чтобы, указать, где произошло изменение нужно использовать состояние.

Особенности `useState`

- Такие компоненты называются "Управляемые"

- Изменения состояние вызывает перерендаринг страницы. Если начальное значения состояние требует каких, то вычислений, то следует указать их стрелочной функцией, тогда эта функция отработает один раз

    ```ts
    const [<ИмяПеременной>, <ФункцияДляИзмененияПеременной>] = useState<ТипЗначений>(() => {
    	// Действия при монтироавние компанента
    	return <Функция>
    });
    ```

- Изменение состояния происходить асинхронно. Поэтому значение переменной состояния, может отставать от реального хода выполнения программы. Для того чтобы получать актуальное значение состояние, нужно использовать стрелочную функцию

    ```ts
    <ФункцияДляИзмененияПеременной>(prev => prev <ДействияС_Переменной>)
    // setCount(prev => prev + 1)
    ```

---

- Для `input` нужно переопределить атрибут `onChange`

```tsx
import React, { useState } from "react";
import './index.css';

const App : React.FunctionComponent = () => {
	// Простой пример состояния
	const [count, setCount] = useState<number>(99)
	// Пример для `input`
	const [text, setText] = useState<string>('Hello')
	// Пример перебора массива с помощью `map`
	const [arr_post, setPost] = useState<(string | number)[][]>(
		[
			[1, "Название 1"],
			[2, "Название 2"],
			[3, "Название 3"],
			[4, "Название 4"],
		]
	)

	return (
		<div className='box_main'>


			{/*//////////*/}
			<div>
				{count}
				<br/>
				<button onClick={() => setCount(count + 1)}>
					+
				</button>
				<br/>
				<button onClick={() => setCount(count - 1)}>
					-
				</button>
			</div>
			{/*//////////*/}
			<div>
				{text} <br/>
				<input type="text" value={text} onChange={
					(event : React.ChangeEvent<HTMLInputElement>) => {
						setText(event.target.value)
					}
				}/>
			</div>
			{/*//////////*/}
			<div>
				{arr_post.map(pos =>
					<h1>{pos[0]} ; {pos[1]} ; {pos[2]} </h1>
				)}
			</div>


		</div>
	);
}

export default App;
```

В виде класса (Классы это устаревший вариант создания)

```jsx
class ClassBasket extends React.Component {
	constructor(props) {
		super(props);

		this.state = {
			con: 87,
		};

		this.increment = this.increment.bind(this);
		this.decrement = this.decrement.bind(this);
	}

	increment() {
		this.setState({ con: this.state.con + 1 });
	}

	decrement() {
		this.setState({ con: this.state.con - 1 });
	}

	render() {
		return (
			<div>
				<span>{this.state.con}</span>
				<br />
				<button onClick={this.increment}>+</button>
				<br />
				<button onClick={this.decrement}>-</button>
			</div>
		);
	}
}
```

### `useRef` - Получать прямой доступ к элементу

```js
const <ИмяПеременной> = useRef(<ЗначениеПоУмолчанию>)
```

Особенности`useRef` [+](https://ru.reactjs.org/docs/hooks-reference.html#useref):

- Изменение объекта не вызовет перерендарега страницы !!!
- При передаче ссылки в компонент нужно использовать специальный синтаксис
- Если указать в атрибут `ref={}` то в `useRef` будет находиться `HTML` тег

---

```js
import "./App.css";
import React, { useRef } from "react";

function App() {
	const body = useRef();

	function logs(vars) {
		console.log(vars);
		vars.current.value = 1 + parseInt(vars.current.value);
	}

	return (
		<div className="box_main">
			<input
				type="button"
				ref={body}
				onClick={() => logs(body)}
				value={"0"}
			/>
		</div>
	);
}
export default App;
```

---

Передача ссылке в компанент:

- Компонент нужно обернуть в функцию ` React.forwardRef( ... )`
- Такие компоненты называются "Не Управляемые"

`App.js`

```js
import "./App.css";
import React, { useRef } from "react";
import Basket from "./basket/Basket";

function App() {
	const body = useRef();
	return (
		<div className="box_main">
			<input
				value={"Нажми на меня"}
				type="button"
				onClick={() => {
					console.log(body.current.value);
					body.current.value = parseInt(body.current.value) + 1;
				}}
			/>

			<Basket ref={body} val={"123"} />
		</div>
	);
}

export default App;
```

`Компанент.js`

```js
import React from "react";

const Basket = React.forwardRef((props, ref) => {
	console.log("Basket");
	console.log(props);

	return (
		<div>
			<input type="text" value={"0"} ref={ref} />
		</div>
	);
});

export default Basket;
```

### `useMemo` - Кешировать результат функции

Эта функция вызваться только когда изменяются значения `ПеременнаяСостаяния`. Это происходит потом что она, кеширует результат `Функция`, и вызовет её только в том случае, если поменяются значение в переменных состояния.

```js
// Кешировать результат функции
const <Перменная> = useMemo(() => { <Функция> }, [ПеременнаяСостаяния_1, ПеременнаяСостаяния_...]);
// Кешировать объект
const <Перменная> = useMemo(() => { <Обьект> }, [ПеременнаяСостаяния_1, ПеременнаяСостаяния_...])
```

---

```js
import React, { useMemo, useState } from "react";

const App = () => {
	const [A, setA] = useState(10);
	const [C, setC] = useState("-");
	const [B, setB] = useState(4);
	const [Result, setResult] = useState(0);

	// Функция будет вызваться при изменение значений (A,B,C)
	const TestUseMem = useMemo(() => {
		console.log("!: UseMem");
		calculate(A, B, C);
	}, [A, C, B]);

	// Функция для подсчета значений
	function calculate(_a, _b, _c) {
		_a = parseInt(_a);
		_b = parseInt(_b);
		console.log(`${_a},${_b},${_c}`);

		let _res = 0;
		switch (_c) {
			case "-":
				_res = _a - _b;
				break;
			case "+":
				_res = _a + _b;
				break;
			case "*":
				_res = _a * _b;
				break;
			case "/":
				_res = _a / _b;
				break;
			case "%":
				_res = _a % _b;
				break;
		}
		console.log(_res);
		setResult(_res);
	}

	return (
		<div>
			<h1>Hello React + Django + !</h1>
			<input
				value={A}
				type="text"
				size={10}
				onChange={(event) => {
					setA(
						parseInt(event.target.value)
							? parseInt(event.target.value)
							: 0
					);
					// calculate(event.target.value, B, C);
				}}
			/>
			<input
				value={C}
				type="text"
				size={1}
				onChange={(event) => {
					setC(event.target.value);
					// calculate(A, event.target.value, C);
				}}
			/>
			<input
				value={B}
				type="text"
				size={10}
				onChange={(event) => {
					setB(
						parseInt(event.target.value)
							? parseInt(event.target.value)
							: 0
					);
					// calculate(A, B, event.target.value);
				}}
			/>
			<br />
			<label> Результат </label> <br />
			<input
				value={Result}
				type="text"
				disabled
				style={{ background: "#FFF" }}
				size={31}
				onChange={(event) => {
					setResult(
						parseInt(event.target.value)
							? parseInt(event.target.value)
							: 0
					);
				}}
			/>
		</div>
	);
};

export default App;
```

### `useEffect` - Рендеринг компонента

Монтирование - рендеринг
Демонтированние - удаление

`useEffect` если не передавать аргументов то, вызовет указанную функцию единожды при первичной отрисовки компонента.

```js
useEffect(() => {
	// Действия при монтироавние компанента
	<Функция>
	return () => {
	//  Действия при демонтирование компанента
	}
}, []);
```

Если передать аргументы, то функция будет вызваться при изменение значения переменной. Поже на `useMemo`, но он не кеширует результат.

```js
const <Перменная> = useEffect(() => {

	<Функция>
	return () => {
	//  Действия при демонтирование компанента
	}
}, [ПеременнаяСостаяния_1, ПеременнаяСостаяния_...]));
```

---

```js
import React, { useEffect, useMemo, useState } from "react";

const App = () => {
	const [A, setA] = useState(10);

	useEffect(() => {
		console.log("Effect");
		return () => {
			console.log("DEL Effect");
		};
	}, []);

	return (
		<div>
			<input
				value={A}
				type="text"
				size={10}
				onChange={(event) => {
					setA(
						parseInt(event.target.value)
							? parseInt(event.target.value)
							: 0
					);
				}}
			/>
		</div>
	);
};

export default App;
```

### `useCallback` - Кешировать ссылку на функцию

Отличие от `useMemo` в том что `useCallback` возвращает ссылку на функцию, а не результат.

```ts
const <ИмяФункции> = useCallback(() => { <Функция> }, [ПеременнаяСостаяния_1, ПеременнаяСостаяния_...]);
```

### `useContext` - Глобальные данные между компонентами

Контекст разработан для передачи данных, которые можно назвать «глобальными» для всего дерева React-компонентов [+](https://ru.reactjs.org/docs/context.html)

- `Context.Provider` = Главный компонент который задает изначальное значение
- `Context.Consumer` = Подчиненный компонент который желает изменить значение
- `useContext` = Получить данные из указанного контекста

```ts
export interface $ИмяИнтерфейсаКонтекста$ {
	$name_1 : string;$
	$name_2 : number;$
	$name_... : ...;$

}

const $ИмяКонтекста$ = React.createContext<AppContextInterface | $ТипИзначальногоЗначения$>($ИзначальноеЗначение$)
```

---

`Context.tsx` Нужно создать отдельный файл с компонентом

```tsx
import React from "react";

export interface $ИмяИнтерфейсаКонтекста$ {
	name : string;
	author : string;
	url : string;
}

const $ИмяКонтекста$ = React.createContext<$ИмяИнтерфейсаКонтекста$ | null>(null)

export default $ИмяКонтекста$
```

`App.tsx`

```tsx
import React,  from "react";
import $ИмяКонтекста$, { $ИмяИнтерфейсаКонтекста$ } from "./Context";
import Tmp from "./components/tmp";

const App : React.FunctionComponent = () => {


	const sampleAppContext: $ИмяИнтерфейсаКонтекста$ = {
		name: "Петя",
		author: "Книга",
		url: "http://www.example.com",
	};

	return (
		<div>
			// Задаем изначальные данные
			<$ИмяКонтекста$.Provider value={sampleAppContext}>
				<Tmp/>
			</$ИмяКонтекста$.Provider>
		</div>
	);
}

export default App;
```

`Tmp1.tsx`

```tsx
import * as React from 'react';
import { useContext } from "react";
import $ИмяКонтекста$, { AppContextInterface } from "../Context";
import Tmp2 from "./tmp2";


type Props = {};

const Tmp = () => {
	// Получить значение из указанного контекста
	const {name, author, url} : $ИмяИнтерфейсаКонтекста$ = useContext($ИмяКонтекста$)!

	return (
		<div>
			// Например хотим изменить данные
			<$ИмяКонтекста$.Consumer>
				{value => value!['name'] = "Костя"}
			</$ИмяКонтекста$.Consumer>
			{name}   // Петя
			{author} // Книга
			{url}    // http://www.example.com

			<Tmp2/>  // Вызываем нижестоящий компонент
		</div>
	);
};
export default Tmp
```

`Tmp2.tsx` - Ключ `name` изменил значение на те что указанны в `Consumer`

```tsx
import * as React from 'react';
import { useContext } from "react";
import $ИмяКонтекста$, { AppContextInterface } from "../Context";
import Tmp2 from "./tmp2";


type Props = {};

const Tmp = () => {

	const {name, author, url} : $ИмяИнтерфейсаКонтекста$ = useContext(TmpContext)!

	return (
		<div>
			{name}   // Костя
			{author} // Книга
			{url}    // http://www.example.com
		</div>
	);
};
export default Tmp
```

### `uesReducer` - Общий обработчик состояния

Вместо того чтобы использовать несколько `useState` можно использовать одну функцию обработчик `useReducer`

```tsx
import React, { useReducer } from "react";

// Тип обьекта для изменниения `useReducer`
interface IAction {
	type : string
}

// Тип изначальных данных
interface IInit {
	name : string,
	counts : number
}

// Констркутор `useReducer`
function init(initialState : IInit | null) {
	if (!initialState) {
		return {name: 'Go', counts: 1}
	}
	return initialState
}

// Функция обработчик событий `useReducer`
function reducer(state : IInit, action : IAction) : any {
	switch (action.type) {
		case 'add_counts':
			return {...state, counts: state.counts += 1}
		case 'change_name':
			return {...state, name: state.name + state.counts}

		case 'reset':
			return init(null)
		default:
			return state;
	}
}

const App : React.FunctionComponent = () => {
	const [state, dispatch] = useReducer(reducer, {name: 'Go', counts: 1}, init)
	return (
		<div>
			<input type="button" onClick={() => dispatch({type: 'add_counts'})} value={"Добавить значение"}/>
			<input type="button" onClick={() => dispatch({type: 'change_name'})} value={"Изменить имя"}/>
			<input type="button" onClick={() => dispatch({type: 'reset'})} value={"Сбросить"}/>
			<hr/>
			{state.name} <br/>
			{state.counts}
		</div>
	);
}

export default App;
```

## React-Bootsrap

- [Документация](https://react-bootstrap.github.io/getting-started/introduction)

Установка в `React`

```bash
npm install react-bootstrap bootstrap --save
```

Подключить `css` файл в проект `index.tsx`

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
// Подключаем `Bootsrap`
import 'bootstrap/dist/css/bootstrap.min.css';
// Подключаем ваши стили
import '../static/frontend_react/style.css'
import './style/index.css';

ReactDOM.render(
	<React.StrictMode>
			<App/>
	</React.StrictMode>,
	document.getElementById('root')
);
```

Использование

```tsx
import React from 'react';

import { Col, Container, Row } from "react-bootstrap";


function HomePage(props : any) {
	return (
		<div>
			<Container>
				<Row>
					<Col>1 of 2</Col>
					<Col>2 of 2</Col>
				</Row>
				<Row>
					<Col>1 of 3</Col>
					<Col>2 of 3</Col>
					<Col>3 of 3</Col>
				</Row>
			</Container>
		</div>
	);
}

export default HomePage;
```

## Многостраничный сайт (Роутинг страниц)

### Введение

- [Документация](https://v5.reactrouter.com/web/guides/quick-start)
- [Видео](https://www.youtube.com/watch?v=0auS9DNTmzE&list=PLiZoB8JBsdznY1XwBcBhHL9L7S_shPGVE)

---

Установить `react-router-dom`

```tsx
npm install --save react-router-dom
```

Если возникнет ошибка с файлом `node_modules/react-router/index.d.ts` то переименуйте его в `node_modules/react-router/index.d.tsx`

---

Настройка

`index.tsx`

```ts
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './style/index.css';
import { BrowserRouter } from "react-router-dom";


ReactDOM.render(
	<React.StrictMode>
		<BrowserRouter>
			<App/>
		</BrowserRouter>
	</React.StrictMode>,
	document.getElementById('root')
);
```

`App.tsx`

- `NavLink to="$URL$"` = Ссылка
- `Routes` = Объединение обработчиков маршрута
- `Route path="$URL$ element=<$Компонент$ />` = Обработчик маршрута
- `useNavigate` = Для навигации по истории ссылок
- `useLocation` = работа с `URL`
- `useSearchParams` = работа с `GET` параметрами
- Можно создавать вложенные роутеры

```tsx
import React, { useReducer } from "react";

import { Route, Routes, Link, NavLink, useNavigate, useLocation, useSearchParams } from "react-router-dom"
import CategoryPage from "./components/CategoryPage";
import HomePage from "./components/HomePage";
import Headers from "./components/Header";


const App : React.FunctionComponent<{ RootUrl : string }> = ({RootUrl}) => {
	return (
		<div>
			Главный url "{RootUrl}" <br/>

			{/*  Вложенная маршрутизация. Путь из вышестояжего `url` будет дописан к нижестоящим */}
			<Routes>
				<Route path={RootUrl} element={<Headers RootUrl={RootUrl}/>}>
					<Route path="home" element={<HomePage/>}/>
					<Route path="category/:id" element={<CategoryPage/>}/>
				</Route>
			</Routes>

		</div>
	);
}

export default App;
```

`Headers` = заголовок страницы. Он будет отображаться на всех страницах

- `Outlet` указывает где отрендарить полезные данные

```tsx
import React from 'react';
import { NavLink, Outlet, useLocation, useNavigate, useSearchParams } from "react-router-dom";

const Headers : React.FunctionComponent<{ RootUrl : string }> = ({RootUrl}) => {
	// Хук для навигации по истории переходов по сслыкам
	const navigate = useNavigate();


	// Полочить текущий `URL`
	let {pathname, ...GetParams} = useLocation();


	// Получить `GET` праметер, и функция для изменения `GET` параметров
	const [getUrl, setGet] = useSearchParams()
	// Получить значение конкретного ключа из `URL`
	console.log("get ", getUrl.get('keys'))
	function paramsToObject(__get : URLSearchParams) {
		const entries : IterableIterator<[string, string]> = __get.entries()
		const result : { [key : string] : string } = {}
		for (const [key, value] of entries) {
			result[key] = value;
		}
		return result;
	}
	// Преобразовать `GET` запрос в `JS` объект
	console.log(paramsToObject(getUrl))


	return (
		<div>
			{/* Так как у нас сохранился контекст `URl` то нам не нужно указывать точный путь,
			можно указать относительный*/}
			<NavLink to={`$home`}>Дом</NavLink> <br/>
			<NavLink to={`$category/1`}>Категория {getUrl.get('id')}</NavLink> <br/>
			<hr/>

			{/* Сюда вставятся данные из странице  */}
			<Outlet/>

			<hr/>
			<input type="button" value="Изменить Get пармитер" onClick={() => setGet({keys: "333"})}/>
			<input type="button" value="Вернутся назад" onClick={() => navigate(-1)}/>
			<input type="button" value="Перейти в корень проекта" onClick={() => navigate('/', {replace: true})}/>
			<footer>***</footer>
		</div>
	);
}

export default Headers;
```

`./components/CategoryPage.tsx`

```tsx
import React from 'react';
import { useParams, useSearchParams } from "react-router-dom";
function CategoryPage(props : any) {
	{/* Получить параметер */}
	const {id} = useParams()
	return (
		<div>Категория {id}</div>
	);
}
export default CategoryPage;
```

`./components/HomePage.tsx`

```tsx
import React from 'react';

function HomePage(props : any) {
	return (
		<div>HomePage</div>
	);
}

export default HomePage;
```

---

### Динамический `Url`

- Получить любое динамическое значение из `url` ( `http:127.0.0.1:8080/product/234`)

    ```tsx
    <Route path={`product/:id`} element={<$Компанент_1$/>}/>
    ```

- Обрабатывать любой `url` после указанного символа ( `http:127.0.0.1:8080/product/phone/234`)

    ```ts
    <Route path={`product/*`} element={<$Компанент_1$/>}/>
    ```

---

Для того чтобы передать динамический параметр нужно использовать следующий синтаксис:

```tsx
import React, { useReducer } from "react";
import { Route, Routes, Link, NavLink } from "react-router-dom"

const App : React.FunctionComponent<{ RootUrl : string }> = ({RootUrl}) => {

	return (
		<Routes>
			<Route path={`${RootUrl}/:id`} element={<$Компанент_1$/>}/>
			<Route path={`${RootUrl}/:id/:category`} element={<$Компанент_2$/>}/>
		</Routes>

);}
export default App;
```

Для того чтобы получить динамический параметр в компоненте необходимо использовать хук `useParams`:

```tsx
import React from 'react';
import {useParams} from 'react-router-dom'

function $Компанент_1$(props : any) {
	console.log(useParams()) // {id:$Значение$}
	return (
		<div> ... </div>
	);
}

export default Page1;
```

## Объединение `Django` + `React`

Наша структура проекта

- `Proj`
    - `Nmae_Django_proj`
    - `Name_React_proj`
    - `Name_Django_App_1`
    - `Name_Django_App_2`
    - `Name_Django_App_...` (и так далее)
    - `static`

---

1. Добавить пути для поиска шаблонов`Name_proj->settings.py->TEMPLATES->'DIRS'`, И добавить путь к статическим файлам `React`

```python
TEMPLATES = [
		{
				'BACKEND' : 'django.template.backends.django.DjangoTemplates',

				'DIRS'    : [os.path.join(BASE_DIR, '<ИмяReactПроекта>/build') ], # !!!!!

				'APP_DIRS': True,
				'OPTIONS' : {
						'context_processors': [
								'django.template.context_processors.debug',
								'django.template.context_processors.request',
								'django.contrib.auth.context_processors.auth',
								'django.contrib.messages.context_processors.messages',
						],
				},
		},
]

# URL-адрес для использования при обращении к статическим файлам, расположенным в STATIC_ROOT.
STATIC_URL = '/static/'
# Путь к общей статической папки.
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
# Список нестандартных путей используемых для сборки.
STATICFILES_DIRS = [
		os.path.join(BASE_DIR, "<ИмяReactПроекта>/build/static"), # !!!!!
]
```

1. Выполнить сборку проекта (тогда появится папка `build`)

    ```bash
    npm run build
    ```

---

### Авто перезагрузка сервера `React`

Наша цель создать приложение для `React` который будет находиться в `Django` проекте, и еще нам нужно автоматически пересобирать `React` при изменении его файлов.

---

1. Инициализировать `npm`

    ```bash
    npm init
    ```

2. Установить `webpack` для автоматической компиляции проекта при изменении файлов в `React` (Для режима разработчика)

    ```bash
    npm install webpack webpack-cli webpack-dev-server --save-dev
    ```

    Добавить в файл `package.json` команду запуска

    ```json
      "scripts": {
    	// Зпускать серве с автокомпиляцией при изменние фалов `React`. Выполнить команду  `npm run dev`
    	"dev": "webpack-dev-server --config webpack.config.js"
      },
    ```

3. Установка `babel`

    ```bash
    npm install @babel/core @babel/preset-env @babel/preset-react babel-loader --save-dev
    ```

4. Установка `React`
    ```bash
    npm install react react-dom jquery --save
    ```

---

Итоговая структура проекта

- `webpack.config.js` (Настройки для компиляции `React`) [Про `webpack` ](https://webpack.js.org/concepts/configuration/)

    ```js
    const path = require("path");
    // Имя с проектом `Django`
    const ProjName = "<ИмяПроекта>";

    module.exports = {
    	// Режим работы
    	mode: "development",

    	entry: [path.resolve(__dirname, `${ProjName}/frontend/src/index.js`)],
    	output: {
    		// куда помещаются скомпилированные файлы
    		path: path.resolve(
    			__dirname,
    			`${ProjName}/frontend/static/frontend/public/`
    		),

    		// 127.0.0.1/static/frontend/public/ откуда подаются файлы
    		publicPath: "/static/frontend/public/",
    		filename: "main.js", // имя для импорта в `index.html`
    	},
    	module: {
    		// конфигурация относительно модулей
    		rules: [
    			{
    				// проверка `regex` для `js` и `jsx` файлов
    				test: /\.(js|jsx|mjs)?$/,
    				// Игнорирует папки [node_modules/, bower_components/]
    				exclude: /(node_modules|bower_components)/,
    				// для поиска подходящих файлов используйте  `babel-loader`
    				use: {
    					loader: "babel-loader",
    					options: {
    						presets: ["@babel/env", "@babel/preset-react"],
    					},
    				},
    			},
    		],
    	},
    	devServer: {
    		devMiddleware: {
    			// Записывать изменения в файл, а не в ОЗУ
    			writeToDisk: true,
    		},
    	},
    };
    ```

- `package.json` (Настройки `npm`, скрипты запуска / зависимости )

    ```json
    {
    	"name": "<Имяпроекта>",
    	"version": "<ВерсияПроекта(1.0.0)>",
    	"description": "<ОписаниеПроекта>",
    	// Имя главного `js` скрипта по умолчанию `index.js`
    	"main": "index.js",
    	"scripts": {
    		"test": "echo \"Error: no test specified\" && exit 1",
    		// Зпускать серве с автокомпиляцией при изменние фалов `React`. Выполнить команду  `npm run dev`
    		"dev": "webpack-dev-server --config webpack.config.js"
    	},
    	// Информация о авторе
    	"author": "<ИмяАвтора>",
    	// Лицензия проекта
    	"license": "MIT",

    	// Зависемости для разработки
    	"devDependencies": {
    		"@babel/core": "^7.16.5",
    		"@babel/preset-env": "^7.16.5",
    		"@babel/preset-react": "^7.16.5",
    		"babel-loader": "^8.2.3",
    		"webpack": "^5.65.0",
    		"webpack-cli": "^4.9.1",
    		"webpack-dev-server": "^4.6.0"
    	},

    	// Продакшин зависемости
    	"dependencies": {
    		"react": "^17.0.2",
    		"react-dom": "^17.0.2"
    	}
    }
    ```

- `Name_Django_Proj`

    - `manage.py`
    - `Name_Django_Proj`
    - `Name_App_Django`
    - `frontend` (Наш `React` проект)

        - `src` (Папка для `React` компонентов)

            - `App.js` (Главный компонент)

                ```js
                import React from "react";

                const App = () => {
                	return <div>Hello React</div>;
                };

                export default App;
                ```

            - `index.js` (Главный скрипт запуска `React`)

                ```js
                import React from "react";
                import ReactDOM from "react-dom";
                import App from "./App";

                ReactDOM.render(
                	<React.StrictMode>
                		<App />
                	</React.StrictMode>,
                	document.getElementById("root")
                );
                ```

        - `static`

            - `frontend`
                - `public` (В эту папку будет компилироваться `React` приложение)
                - `style.css` (Главные `css` стили)

        - `templates`

            - `frontend`

                - `index.html` (Главный `html` шаблон)

                    ```html
                    {% load static %}
                    <!DOCTYPE html>
                    <html>
                    	<head>
                    		<meta charset="utf-8" />
                    		<meta
                    			name="viewport"
                    			content="width=device-width, initial-scale=1"
                    		/>
                    		<link
                    			rel="stylesheet"
                    			href="{% static 'frontend/style.css' %}"
                    		/>
                    		<title>React + Django</title>
                    	</head>
                    	<body>
                    		<div id="root" class="content"></div>
                    		<script
                    			type="text/javascript"
                    			src="{% static 'frontend/public/main.js' %}"
                    		></script>
                    	</body>
                    </html>
                    ```
