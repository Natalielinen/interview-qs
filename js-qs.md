# Вопросы по JS

## Оглавление
1. [Как работают замыкания (closures) в JavaScript?](#closures)
2. [Объясните разницу между var, let и const](#variables)
3. [Что такое область видимости (scope) в JavaScript?](#scope)
4. [Как работает Event Loop в JavaScript?](#eventLoop)
5. [Чем отличаются setTimeout, setInterval и requestAnimationFrame?](#timeout)
6. [Что такое "поверхностное" и "глубокое" копирование объектов?](#deepShallowCopy)
7. [Какие есть способы реализации наследования в JavaScript?](#inheritance)
8. [Как работает прототипное наследование?](#prototypes)
9. [В чем разница между == и ===?](#equals)
10. [Как работает всплытие (event bubbling) и погружение (event capturing)?](#bubbling)
11. [Деструктуризация — удобный способ извлекать значения из объектов и массивов.](#destructuring)
12. [Map и Set. ](#mapset)
13. [async/await ](#asyncawait)
14. [Генераторы (function*)](#generators)
15. [Стрелочные функции ](#arrowFunctions)
16. [Хранение данных в браузере (localStorage, sessionStorage, cookies )](#storage)
17. [Debounce & Throttle](#debounce)
18. [Call, bind, apply](#callBindApply)
19. [Каррирование](#curry)


## <a id="closures">Как работают замыкания (closures) в JavaScript?</a>

**Ответ**: Замыкание — это функция, которая запоминает свою внешнюю область видимости, даже если она вызывается вне этой области.

```js
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
  };
}

const closureExample = outerFunction("Hello");
closureExample("World"); // Выведет: Outer: Hello, Inner: World

 ```

 **Где используется**: Callback-функции, таймеры, модули, функции с приватными переменными.

 ## <a id="variables">Объясните разницу между var, let и const</a> 

 | Переменная | Область видимости |	Изменяемость |	Возможность повторного объявления |
 |------------|-------------------|--------------|------------------------------------|
 | var	      | Функциональная	  | Можно менять |	Можно объявить повторно           |
 | let        |	Блочная           |	Можно менять |	Нельзя объявить повторно          |
 | const      |	Блочная           |	Нельзя менять|	Нельзя объявить повторно          |

 ```js
 var x = 10;
if (true) {
  var x = 20; // Затрет предыдущее значение x
}
console.log(x); // 20

let y = 10;
if (true) {
  let y = 20; // Переменная ограничена блоком
}
console.log(y); // 10

  ```

 ## <a id="scope">Что такое область видимости (scope) в JavaScript?</a> 

 **Ответ**: Область видимости определяет, откуда можно получить доступ к переменным.

- Глобальная

- Функциональная

- Блочная (для let и const)

```js
let globalVar = "Я глобальная";
function test() {
  let functionVar = "Я внутри функции";
  if (true) {
    let blockVar = "Я внутри блока";
  }
  console.log(functionVar); // Работает
  console.log(blockVar); // Ошибка
}
console.log(globalVar); // Работает

 ```

 ## <a id="eventLoop">Как работает Event Loop в JavaScript?</a>

 **Ответ**: JavaScript — однопоточный язык, но с асинхронностью. Event Loop контролирует стек вызовов и очередь задач.

- Stack (Стек) — выполняет синхронный код.

- Web API — обрабатывает асинхронные операции (setTimeout, fetch).

- Callback Queue — очередь коллбэков из Web API.

- Event Loop — переносит задачи из очереди в стек.

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Вывод:
// Start
// End
// Promise
// Timeout

 ```

 ## <a id="timeout">Чем отличаются setTimeout, setInterval и requestAnimationFrame?</a>

- setTimeout(callback, delay) – выполняет код 1 раз через указанное время.

- setInterval(callback, interval) – выполняет код многократно через указанный интервал.

- requestAnimationFrame(callback) – выполняет код перед перерисовкой кадра (60 раз в секунду).

```js
setTimeout(() => console.log("Через 2 секунды"), 2000);

let count = 0;
let interval = setInterval(() => {
  console.log(`Прошло ${++count} сек`);
  if (count === 5) clearInterval(interval);
}, 1000);

function animate() {
  console.log("Анимация кадра");
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);

 ```

 ## <a id="deepShallowCopy">Что такое "поверхностное" и "глубокое" копирование объектов?</a>

- Поверхностное копирование копирует только верхний уровень объекта.

- Глубокое копирование копирует вложенные структуры.

```js
const obj1 = { a: 1, b: { c: 2 } };
const shallowCopy = { ...obj1 };
shallowCopy.b.c = 42; // Изменит исходный объект

const deepCopy = JSON.parse(JSON.stringify(obj1)); // Глубокая копия

 ```

 ## <a id="inheritance">Какие есть способы реализации наследования в JavaScript?</a>

- Через prototype

- Через class (ES6)

- Через Object.create()

```js
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
}

 ```

 ## <a id="prototypes">Как работает прототипное наследование?</a>

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function () {
  console.log(`Hello, my name is ${this.name}`);
};

const user = new Person("Alice");
user.sayHello(); // Hello, my name is Alice

 ```

Объекты наследуют методы через цепочку прототипов (__proto__).

## <a id="equals">В чем разница между == и ===?</a>

- == — нестрогое сравнение, преобразует типы.

- === — строгое сравнение, не преобразует.

```js
console.log(5 == "5"); // true
console.log(5 === "5"); // false

 ```

## <a id="bubbling">Как работает всплытие (event bubbling) и погружение (event capturing)?</a>

```js
document.body.addEventListener(
  "click",
  () => console.log("Body"),
  true // Режим захвата (capturing)
);
document.getElementById("child").addEventListener("click", () => console.log("Child"));

 ```

Всплытие: от дочернего элемента к родителю.
Погружение: от родителя к дочернему.

## <a id="destructuring">Деструктуризация — удобный способ извлекать значения из объектов и массивов.</a>

```js
const { name, age } = { name: "Alice", age: 25 };
const [arr1, arr2] = [1, 2, 3] // arr1 = 1, arr2 = 2
 ```

## <a id="mapset">Map и Set.</a> 

**Map** – это коллекция ключ/значение, как и Object. Но основное отличие в том, что Map позволяет использовать ключи любого типа.

```js
let map = new Map();

map.set("1", "str1");    // строка в качестве ключа
map.set(1, "num1");      // цифра как ключ
map.set(true, "bool1");  // булево значение как ключ

// помните, обычный объект Object приводит ключи к строкам?
// Map сохраняет тип ключей, так что в этом случае сохранится 2 разных значения:
alert(map.get(1)); // "num1"
alert(map.get("1")); // "str1"

alert(map.size); // 3
 ```
**Map может использовать объекты в качестве ключей.**

Объект **Set** – это особый вид коллекции: «множество» значений (без ключей), где каждое значение может появляться только один раз.

```js
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// считаем гостей, некоторые приходят несколько раз
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// set хранит только 3 уникальных значения
alert(set.size); // 3

for (let user of set) {
  alert(user.name); // John (потом Pete и Mary)
}
 ```
Альтернативой множеству Set может выступать массив для хранения гостей и дополнительный код для проверки уже имеющегося элемента с помощью arr.find.

## <a id="asyncawait">async/await</a> 

Существует специальный синтаксис для работы с промисами, который называется «async/await». Он удивительно прост для понимания и использования.

Ключевое слово async перед функцией гарантирует, что эта функция в любом случае вернёт промис

Ключевое слово await заставит интерпретатор JavaScript ждать до тех пор, пока промис справа от await не выполнится. После чего оно вернёт его результат, и выполнение кода продолжится.

```js
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("готово!"), 1000)
  });

  let result = await promise; // будет ждать, пока промис не выполнится (*)

  alert(result); // "готово!"
}

f();
 ```

## <a id="generators">Генераторы (function*)</a>

Обычные функции возвращают только одно-единственное значение (или ничего).

Генераторы могут порождать (yield) множество значений одно за другим, по мере необходимости. Генераторы отлично работают с перебираемыми объектами и позволяют легко создавать потоки данных.

Для объявления генератора используется специальная синтаксическая конструкция: function*, которая называется «функция-генератор».

```js
function* generateSequence() {
  yield 1;
  yield 2;
  return 3;
}
 ```

## <a id="arrowFunctions">Стрелочные функции</a>

Стрелочная функция это короткая запись функционального выражения (function expression) без собственных привязок this, arguments и super.

У стрелочных функций нет своих привязок для arguments, super, this или new.target. Значения этих ключевых слов привязываются к внешнему лексическому окружению.

Использование call() или apply() никак не влияет на стрелочные функции.

## <a id="storage">Хранение данных в браузере (localStorage, sessionStorage, cookies )</a>

**localStorage** 

[localStorage](https://doka.guide/js/local-storage/)

Это объект, хранящийся в window, который позволяет долговременно сохранять данные в браузере. Работает как хранилище данных в формате ключ-значение — при сохранении данных мы указываем имя поля, в которое должны быть сохранены данные, и затем используем это имя для их получения.

**sessionStorage**

[sessionStorage](https://doka.guide/js/session-storage/)

Это объект, хранящийся в window, который позволяет сохранять данные в браузере на время сессии. Этот тип хранилища очень похож на localStorage и работает как хранилище данных в формате ключ-значение. При сохранении данных мы указываем имя поля, в которое должны быть сохранены данные, и затем используем это имя для их получения.

- Сессия страницы создаётся при открытии новой вкладки браузера. Сессия остаётся активной до тех пор, пока открыта вкладка, а состояние сессии сохраняется между перезагрузками. Открытие новой вкладки с таким же адресом приведёт к созданию новой сессии.
- Значения хранятся в виде строк. При попытке сохранения других типов данных, они будут приведены к строке. Например, если записать число, то при чтении нам вернётся число, записанное в строку.
- Максимальный объем данных ограничен размером 5MB.

**cookies**

[cookies](https://doka.guide/js/cookie/)

При разработке сайтов часть информации (например, токен авторизации или данные пользователя) нужно хранить и читать как в браузере, так и на сервере. Для этого используют Cookie (произносится «куки»).

>Куки передаются в виде HTTP-заголовка, это накладывает на них ограничения. Например, максимальный размер куки в 4096 байт или отсутствие в содержимом пробелов или запятых. Чтобы обезопасить содержимое, можно закодировать его с помощью функции encodeURIComponent().

## <a id="debounce">Debounce & Throttle</a>

**Debounce (устранение дрожания)**

Debounce позволяет задерживать выполнение функции, пока не пройдет определенное время с момента последнего вызова.
Используется, когда событие вызывается слишком часто (например, ввод в поле поиска).

Пример использования:
- Автодополнение при вводе текста

- Обновление фильтров по мере ввода текста

- Оптимизация запросов на сервер

```js
function debounce(func, delay) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), delay);
  };
}

// Пример использования
const searchInput = document.getElementById("search");

searchInput.addEventListener(
  "input",
  debounce((event) => {
    console.log("Запрос на сервер: ", event.target.value);
  }, 500)
);

```

Как работает:
При каждом вводе символа предыдущий таймер сбрасывается (clearTimeout), и функция вызывается только после завершения паузы.

**Throttle (ограничение частоты вызова)**

Throttle позволяет вызывать функцию не чаще, чем раз в указанный интервал времени.
Используется, когда нужно ограничить частоту вызовов (например, скролл или ресайз).

Пример использования:
- Обработчик скролла (scroll)

- Изменение размеров окна (resize)

- Обновление положения элементов при скролле

```js
function throttle(func, interval) {
  let lastTime = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      func.apply(this, args);
      lastTime = now;
    }
  };
}

// Пример использования
window.addEventListener(
  "scroll",
  throttle(() => {
    console.log("Событие скролла обработано");
  }, 1000)
);

 ```

Как работает:
Функция выполняется не чаще, чем раз в interval миллисекунд, игнорируя остальные вызовы в этом промежутке. 

## <a id="callBindApply">Call, bind, apply</a>

У объектов в JS есть свои свойства и методы. Один объект не может воспользоваться методом другого объекта и наоборот. Эти ограничения позволяют обойти методы bind(), call() и apply(). `Эти методы используются для привязки функции к обекту и позволяют ее вызвать так, будто она принадлежит этому объекту`.

**Метод call()**

Вызывает функцию с заданным контекстом. Т. е. можно привязать функцию к объекту, как если бы она ему принадлежала: 

```js
let obj = {
	num: 2
};

function add(a) {
	return this.num + a;
}
```


Так как в функции add нет свойства num, то вызвать this.num не получится. Но можно привязать объект obj у которого такое свойство есть.


add.call(obj, 3) // первый параметр - объект к которому надо привязать ф-цию
                 // далее перечисляются параметры для самой ф-ции (add)

**Применение метода call() в JS**

Метод call() может использоваться для создания цепочек конструкторов объектов, для вызова анонимной ф-ции (например, в цикле для массива объектов), для выполнения ф-ции с объектом (пример с ф-цией add).

**Метод apply()**

Аналогичен методу call(), отличие в том, что аргументы для привязываемой ф-ции передаются в виде массива, а не через запятую.

```js
let obj = { num: 2 };

function add(a, b) {
	 return this.num + a + b;
}

add.apply(obj, [3, 5]);
```


**Применение метода apply() в JS**

Все то же самое что и у call(), плюс можно использовать для добавления одного массива к другому (используя push()).


const numbers = [1, 2, 3]
const moreNumbers = [4, 5, 6]

numbers.push.apply(numbers, moreNumbers)



**Метод bind()**

Отличается от call() и apply() тем, что возвращает не вычесленное значение, а функцию, которую можно использовать в нужный момент.

```js
let obj = { num: 2 };

function add(a, b) {
	 return this.num + a + b;
}

const func = add.bind(obj, 3, 5);

func();
```

**Применение метода bind() в JS**
bind() применяется для создания привязанной ф-ции. Посредством bind() можно создать ф-цию привязанную к объекту. При этом не имеет значения где и когда она будет вызвана. 


## <a id="curry">Каррирование</a>

Каррирование – это трансформация функций таким образом, чтобы они принимали аргументы не как f(a, b, c), а как f(a)(b)(c)