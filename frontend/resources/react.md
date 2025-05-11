# Hooks
> Это механим или синтаксический сахар, позволяющий работать в React без классов. Т.е. есть возможность сохранять состояния между множественными вызовами, передавать это состояние и инкапсулировать кучу логики в декларативный подход.
## useEffect
```js
useEffect(() => {
	//
    // Код, который выполняется после рендеринга
	//
    return () => {
		//
        // Код, который выполняется при размонтировании компонента
		//
    };
}, [dependencies]);
```

* `[dependencies]` -> Массив зависимостей. Если зависимости изменяются, эффект выполняется заново. Если массив пустой (`[]`), эффект выполняется только один раз 
## useState
```js
import React, { useState } from 'react';

function Counter() {
	//
	// count - та самая возможность сохранения состояния между вызовами
	//
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Вы нажали {count} раз</p>
            <button onClick={() => setCount(count + 1)}>
                Нажми меня
            </button>
        </div>
    );
}	
```
## useContext
> Позволяет передавать какие-то данные по всему дереву.
> Допустим, надо передать данные с самого верхнего компонента на самый нижний, обычно надо прокидывать их на каждом компоненте, но хук контекста позволяет сделать это максимально удобно.
```js
import React, { createContext, useContext } from 'react';

//
// Задаём контекст
//
const ThemeContext = createContext();

function App() {
    return (
		//
        // Передаем значение "dark" в контекст через Provider
		//
        <ThemeContext.Provider value="dark">
            <Toolbar />
        </ThemeContext.Provider>
    );
}

function Toolbar() {
    return (
        <div>
            <ThemedButton />
        </div>
    );
}

function ThemedButton() {
	//
    // Используем useContext для доступа к значению контекста: theme = "dark"
	//
    const theme = useContext(ThemeContext);
    return (
        <button style={{ background: theme === 'dark' ? '#333' : '#CCC' }}>
            Тема: {theme}
        </button>
    );
}
```
## useRef
> Хук, который хранит ссылку на некоторый объект, часто используется там, где надо по ссылке менять значение и не re-render'ить отображение.
```js
import React, { useRef, useEffect } from 'react';

function Timer() {
	//
	// Инициализация
	//
    const timerRef = useRef(0);

    useEffect(() => {
        const interval = setInterval(() => {
			//
			// Таймер тикает, но в качестве своего поля использует хук useRef
			//
            timerRef.current += 1;
            console.log(`Таймер: ${timerRef.current}`);
        }, 1000);

        return () => clearInterval(interval);
    }, []);

    return (
        <div>
            <p>Таймер работает (см. консоль)</p>
        </div>
    );
}
```