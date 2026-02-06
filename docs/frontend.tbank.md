---
title: Т-Банк. Frontend. JavaScript
description: Подготовка к собеседованию в Т-Банк Frontend разработчик. Тест по JavaScript Т-Банк.
---

# Вопросы и ответы

## 1. Что будет выведено в консоль?
```javascript
console.log([1, 2, 3].map((el) => el * 2))
```

- [ ] [1, 2, 3]
- [ ] [4, 8, 12]
- [ ] [2, 4, 6]
- [ ] [0, 2, 4]

## 2. Вы пишете код, необходимо создать переменную, значение которой не планируется изменять. Выберите наиболее подходящий вариант.

- [ ] var
- [ ] let
- [ ] const

## 3. Что нужно изменить в методе add, чтобы в результате получилось 4? 
```javascript
const calc = {
    value: 1,

    add(n) {
        // ?
    },

    multiply(n) {
        this.value *= n;
        return this;
    }

    calc.add(2).multiply(1).add(2);

    console.log(calc.value); // 4
}
```

- [X] Нет правильного ответа

## 4. Как можно модифицировать вызов метода sayHello, чтобы увидеть "Hello, world!" в консоли?
```javascript
function sayHello() {
    console.log(this.message);
}

const greeting = {
    message: "Hello, world!"
}

sayHello
```

- [ ] sayHello.call(greeting)
- [ ] sayHello.apply(greeting)
- [ ] sayHello.bind(greeting)

## 5. Сборка настроена так, что проект собирается даже с ошибками TS. В вашем проекте есть кусок кода. Что произойдет в браузере?
```javascript
let a: number = 1;
a = '1';
```

- [ ] Будет ошибка
- [ ] Все будет работать корректно, а будет строкой '1'
- [ ] Может будет работать, может нет
- [ ] а будет числом 1

## 6. Игнат разрабатывает админ-панель. Как можно отправить запрос на создание новой сущности с помощью стандартного API браузера и следуя REST API?

- [ ] 
```javascript
fetch('/api/entities', { method: 'POST', headers, body });
```
- [ ] 
```javascript
const xhr = new XMLHttpRequest();
xhr.open('POST', '/api/entities');
xhr.setRequestHeader(headers);
xhr.send(body);
```
- [ ] 
```javascript
fetch('/api/entities', { method: 'PUT', headers, body });
```
- [ ] 
```javascript
axios.post('/api/entities', body);
```

## 7. Что выведет следующий код?
```javascript
const obj = { then: () => { console.log('then'); } };
Promise.resolve(obj).then(() => console.log('when'));
```

- [ ] when
- [ ] then
- [ ] then -> when
- [ ] Будет ошибка

## 8. Что будет выведено в консоль?
```javascript
Promise.resolve()
  .then(() => console.log(1))
  .then(() => console.log(2));
console.log(3);
```

- [ ] 1 2 3
- [ ] 3 1 2
- [ ] 1 3 2
- [ ] 3 2 1

## 9. Какой код корректно отменяет выполнение fetch запроса?

- [ ] 
```javascript
fetch(url).cancel();
```
- [ ] 
```javascript
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort();
```
- [ ] 
```javascript
cancel(fetch(url));
```
- [ ] 
```javascript
fetch.abort(url);
```

## 10. Что необходимо дописать в классе .container, чтобы элементы стали в одну строку?

```html
<div class="container">
  <div class="item">A</div>
  <div class="item">B</div>
  <div class="item">C</div>
</div>
```

```css
.container {
  width: 300px;
  /* что дописать? */
}
```

- [ ] display: inline;
- [ ] display: inline-block;
- [ ] display: flex;
- [ ] display: grid;

## 11. Что может быть выведено в консоль браузера?
```html
<html>
<head>
  <script src="a.js" defer></script>
  <script src="b.js" defer></script>
</head>
<body>
  <script>console.log('inline');</script>
</body>
</html>

a.js -> console.log('A');
b.js -> console.log('B');
```

- [ ] A B inline
- [ ] inline A B
- [ ] B inline A
- [ ] inline B A

## 12. Что будет выведено в консоль?

```html
<div id="parent">
  <button id="child">Click me</button>
</div>
<script>
  const parent = document.getElementById('parent');
  const child = document.getElementById('child');
  parent.addEventListener('click', () => console.log('parent'), true);
  child.addEventListener('click', () => console.log('child'));
</script>
```

- [ ] child -> parent
- [ ] parent -> child
- [ ] child
- [ ] parent

## 13. Ваш проект открыт в нескольких вкладках и вы выполняете код в разных вкладках. Что будет выведено?
```javascript
// Вкладка 1
sessionStorage.setItem('key', '1');
console.log(sessionStorage.getItem('key'));

// Вкладка 2
console.log(sessionStorage.getItem('key'));
```

- [ ] 1 -> null
- [ ] 1 -> 1
- [ ] null -> 1
- [ ] null -> null

## 14. Что будет выведено?
```javascript
try {
  throw new Error('Oops!');
} catch {
  console.log('caught');
} finally {
  console.log('finally');
}
```

- [ ] caught
- [ ] caught -> finally
- [ ] finally -> caught
- [ ] Ошибка -> скрипт упадет
