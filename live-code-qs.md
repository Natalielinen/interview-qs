1. [Недоступность переменной](#unavailableVariable)
2. [Деструктуризация](#destructuring)
3. [Что выведется в консоль?](#consoleLogs)
4. [Индекс в доке запрещен в использовании – почему?](#indexNotAllowed)
5. [Какой вызов предпочтительнее и почему?](#callbackcall)
6. [Добавление элементов в массив](#addToArray)
7. [Какое имя выведит привязанная функция?](#bindFunc)
8. [Функция makeCounter](#makeCounter)
9. [Пуш функции в массив](#pushFuncToArr)
10. [Ошибка обязательный параметр](#errorParamIsRequired)
11. [isNan](#isNan)
12. [class Test](#classTest)
13. [Промисы](#promise)
14. [foo try/catch](#fooTryCatch)
15. [static](#15)
16. [Symbol.iterator](#16)
17. [fetchData](#17)
18. [Product.prototype](#18)
19. [Leonardo](#19)
20. [Promise](#20)
21. [setTimeout](#21)
22. [obj a b](#22)
23. [Set](#23)
24. [objects compare](#24)

## <a id="unavailableVariable">Что нужно сделать с первой строчкой, чтобы внутри функции b переменная a стала недоступна?</a>

```ts
var a = 10;

function b () { a; }
```

**Ответ**:

Чтобы переменная a стала недоступной внутри функции b, её нужно не объявлять в той области видимости, где она доступна функции.

В данном случае переменная a объявлена глобально (var a = 10;), и функция b имеет к ней доступ, потому что она ищет переменные в своём замыкании и затем — в глобальной области видимости.

```ts
{
  var a = 10;
}

function b () { a; } // Не сработает, у var не блочная область

```

Можно обернуть переменную в блок и сделать let или const

## <a id="destructuring">Как реализовать деструктуризацию для данного присвоения, чтобы справа был только user?</a>

```js
const stage = user?.education.stage;


```

```js
const { education: { stage } = {} } = user || {};
```

user || {} — защита от undefined/null в user;

education: { stage } = {} — защита от undefined в user.education

```js
const user = {
  education: {
    stage: 'university'
  }
};

const { education: { stage } = {} } = user || {};
console.log(stage); // 'university'
```

## <a id="consoleLogs">Что выведется в консоль и почему?</a>

```js
var a = {}, b = { key: 'b' }, c = { key: 'c' };

a[b] = c;
a[c] = b;

console.log(a[b]); console.log(a[c]);
```

Когда объект b или c используется как ключ в обычном объекте a, он не остаётся объектом. JavaScript автоматически приводит его к строке — это поведение по умолчанию для объектов в качестве ключей.

```js
String(b) // => "[object Object]"
String(c) // => "[object Object]"
```
```js
a["[object Object]"] = c; // первая строка
a["[object Object]"] = b; // вторая строка перезаписывает значение
```
```js
console.log(a[b]); // { key: 'b' }
console.log(a[c]); // { key: 'b' }
```
"Это классическая ловушка JS, показывающая, что обычные объекты не предназначены для использования в качестве ключей, если важна уникальность. В таких случаях лучше использовать Map, где ключи действительно могут быть объектами."

## <a id="indexNotAllowed">Индекс в доке запрещен в использовании – почему?</a>

❌ Почему использование индекса как key считается плохой практикой?
1. Нарушается логика рендера
React использует key, чтобы понять, какие элементы в списке изменились, удалились или добавились. Если key — это индекс, то при изменении порядка элементов (например, при удалении одного из них) React может неправильно переиспользовать компоненты, что приведёт к багам в UI.

2. Проблемы со стейтом
Если внутри компонента есть useState или useEffect, и key не уникальный (например, просто индекс), то состояние компонента может не сброситься как ожидается, и пользователь увидит старое состояние в другом элементе.

3. Плохая производительность
React будет считать, что компонент с key=0 остался на месте, даже если теперь та

## <a id="callbackcall">Какой вызов предпочтительнее и почему?</a>

```tsx
  <Component cb={this.toggle} />
  <Component cb={() => this.toggle()} />
```

✅ Предпочтительнее первый вариант:

- Функция не создаётся заново при каждом рендере
В первом случае ссылка на this.toggle сохраняется между рендерами. Это важно для оптимизации React — особенно если Component зависит от сравнения пропсов (React.memo, shouldComponentUpdate, PureComponent).

- Меньше нагрузки на GC (сборщик мусора)
В случае стрелочной функции () => this.toggle() — при каждом рендере создаётся новая функция, что потенциально увеличивает нагрузку на сборщик мусора и может вызвать лишние перерендеры.

- Чище код и читаемость выше, если не нужно прокидывать аргументы.

## <a id="addToArray">Добавление элементов в массив</a>

```js
const numbers = [1, 2, 3];
numbers[10] = 11;

console.log(numbers); // [1, 2, 3, ..., 11] будет 11 эл-тов, в массив const добавлять элементы можно!
```

## <a id="bindFunc">Какое имя выведит привязанная функция?</a>

```js
function f() {
  console.log(this.name);
}

f = f.bind({name: 'Вася'}).bind({name: 'Петя'});

f(); // Выведется Вася, т.к. первый bind привяжет this навсегда (второй bind не переопределяет this), call и apply здесь тоже не сработают
```

## <a id="makeCounter">Функция makeCounter</a>

```js
function makeCounter() {
  let count = 0; // Функция makeCounter создаёт замыкание — возвращает внутреннюю функцию, которая имеет доступ к переменной count в своей области видимости:

  return function() {
    return count++;
  }
}

/*
📌 Тут создаются два независимых счётчика:

counter имеет свою переменную count, которая начинается с 0

counter2 тоже имеет свою count, отдельную от counter
*/

const counter = makeCounter();
const counter2 = makeCounter();

counter(); // count = 0 → вернёт 0, затем count = 1
counter(); // count = 1 → вернёт 1, затем count = 2

//Т.е. counter2 вызывается впервые, у него свой count, начинающийся с 0.
console.log(counter2()); // 0
console.log(counter2()); // 1

```

✅ Ответ:

0
1

## <a id="pushFuncToArr">Пуш функции в массив</a>

```js
const arr = ['a', 'b'];

// 📌 Что такое this в данном случае?
// Функция вызывается как метод массива:
arr.push(function() {
    console.log(this)
});

arr[2]();
```

✅ Ответ:

['a', 'b', ƒ] // сам массив arr

## <a id="errorParamIsRequired">Ошибка обязательный параметр</a>
```js
const isRequired = () => {
  throw new Error('param is required');
};

const foo = (num = isRequired()) => {
  console.log(num);
};

foo(42); // 42
foo(null); // null
foo(); // Error: 'param is required'
```

## <a id="isNan">isNan</a>

```js
console.log(isNaN()); // true - 
console.log(isNaN(undefined)); // true
console.log(isNaN({})); // true
console.log(isNaN([])); // false
```

Функция isNaN() определяет, является ли значение NaN, сначала преобразуя значение в число, если это необходимо. Поскольку приведение внутри функции isNaN() может быть неожиданным, вы можете предпочесть использовать Number.isNaN().

## <a id="classTest">class Test</a>

```js
class Test {
  firstFunction () {
    console.log('first');
    this.#secondFunction();
  }

  #secondFunction () {
    console.log('second');
  }
}

const test = new Test();

test.firstFunction();
test.#secondFunction(); // Ошибка
```

📌 Почему?
Метод #secondFunction — это приватный метод (начинается с #) в классе Test.

Важно:
Приватные поля и методы доступны только внутри самого класса.

Любая попытка обратиться к ним извне приводит к синтаксической ошибке (SyntaxError), даже если вы находитесь в том же модуле.

🔍 Что произойдёт при выполнении:
test.firstFunction();
✅ Эта строка выведет:

first
second
потому что метод firstFunction() вызывает приватный #secondFunction() изнутри класса, что разрешено.

Первый вызов сработает, так как приватный метод вызывается внутри класса. Второй вызов выдаст синтаксическую ошибку, потому что приватные методы недоступны снаружи класса. Приватность в JavaScript обеспечивается через #, и попытка доступа к ним извне нарушает спецификацию языка.

## <a id="promise">Промисы</a>

```js
const p1 = new Promise(res => {
  setTimeout(res, 500, 'first');
});

const p2 = new Promise(res => {
  setTimeout(res, 100, 'second');
});

Promise.race([p1, p2]).then(console.log);
```

⚡ Что делает Promise.race?
Promise.race возвращает первый завершившийся промис (независимо от того, был он выполнен успешно или с ошибкой).

➡️ В данном случае p2 завершится раньше (100мс против 500мс), поэтому Promise.race([p1, p2]) выполнится с результатом "second".

Promise.race возвращает результат самого первого завершённого промиса. В этом примере p2 завершается быстрее, чем p1, поэтому в then попадает значение 'second'. Даже если p1 потом завершится успешно — это уже не повлияет на результат race.

Promise.all() - резолвится если все промисы успешны, если хоть один завалился - rejected (Fulfills when all of the promises fulfill; rejects when any of the promises rejects.)
Promise.allSettled() - The Promise.allSettled() static method takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when all of the input's promises settle (including when an empty iterable is passed), with an array of objects that describe the outcome of each promise. (даже если какие-то отвалились)
Promise.any() - Fulfills when any of the promises fulfills; rejects when all of the promises reject.
Promise.race() - Settles when any of the promises settles. In other words, fulfills when any of the promises fulfills; rejects when any of the promises rejects.

## <a id="fooTryCatch">foo try/catch</a>

```js
const foo = () => {
  try {
    throw new Error('error'); // выбрасываем ошибку
    return 24;                // эта строка НЕ выполнится
  } catch (error) {
    return error;             // перехватываем и пытаемся вернуть ошибку
  } finally {
    return 42;                // но finally ПЕРЕЗАПИШЕТ return
  }
}

console.log(foo());
```
📌 Как работает try..catch..finally?
Если в try выбрасывается ошибка, управление передаётся в catch.

Если в catch есть return, он сохраняется…
НО если finally тоже содержит return, он ПЕРЕЗАПИШЕТ любое значение из try или catch.

Несмотря на то что в catch возвращается ошибка, блок finally содержит return 42, а он всегда выполняется последним и переопределяет результат из try или catch. Поэтому foo() возвращает 42. Это особенность JavaScript — return в finally имеет высший приоритет.

## <a id="15">static</a>

```js
class Vehicle {
  static type() {
    return "Generic Vehicle";
  }
}

class Car extends Vehicle {
  static type() {
    return "Car";
  }
}

console.log(Car.type());  //Car
```

В классе Car переопределяется статический метод type, вызвать его можно только из класса, а не из экземпляра. Выведется Car

## <a id="16">Symbol.iterator</a>

```js
const myObject = {
  a: 1,
  b: 2,
  c: 3,
  [Symbol.iterator]: function* () {
    for (let key of Object.keys(this)) {
      yield this[key];
    }
  }
};

const iter = myObject[Symbol.iterator]();
console.log(iter.next().value);
console.log(iter.next().value);
console.log(iter.next().value);
```

Делает обычный объект итерируемым (как массив).

При итерации возвращает значения его строковых свойств (1, 2, 3).

В консоли будет выведено: 1, 2, 3

Но есть важная деталь: Object.keys(this) возвращает только обычные (строковые) ключи, символы не включаются. То есть Symbol.iterator не попадает в список ключей.

Это не псевдомассив, т.к у него нет свойства length

## <a id="17">fetchData</a>

```js
async function fetchData() {
  let data = 'initial';

  const promise = new Promise((resolve) => {
    setTimeout(() => {
      resolve('fetched');
    }, 1000);
  });

  promise.then((result) => {
    data = result;
  });

  return data;
}

fetchData().then(console.log);
```

data изначально — строка 'initial'.

Создаётся Promise, который через 1 секунду вызовет resolve('fetched').

Устанавливается .then(...), в котором будет обновлено значение data → 'fetched'.

Но функция fetchData возвращает data сразу, до того как setTimeout сработает и промис выполнится.

## <a id="18">Product.prototype</a>

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

Product.prototype.discount = function(discount) {
  this.price -= discount;
};

const product = new Product('Phone', 500);
product.discount(50);

console.log(product.price);
```

Создаётся конструктор Product, который инициализирует объект с полями name и price.

Добавляется метод discount в прототип конструктора, чтобы все экземпляры Product могли его использовать. Метод просто уменьшает цену на переданное значение.

Создается экземпляр product с именем 'Phone' и ценой 500.

Затем вызывается метод discount(50), который уменьшает цену на 50.

После этого console.log выводит новое значение цены. (450)

## <a id="19">Leonardo</a>

```js
let person = {
        name: 'Leonardo'
};

Object.freeze(person);
person.name = 'Lima';

console.log(person.name);
```

Помимо значения **`value`**, свойства объекта имеют три специальных атрибута (так называемые «флаги»).

- **`writable`** – если `true`, свойство можно изменить, иначе оно только для чтения.
- **`enumerable`** – если `true`, свойство перечисляется в циклах, в противном случае циклы его игнорируют.
- **`configurable`** – если `true`, свойство можно удалить, а эти атрибуты можно изменять, иначе этого делать нельзя.

Запрещает добавлять/удалять/изменять свойства. Устанавливает `configurable: false, writable: false` для всех существующих свойств.

## <a id="20">Promise</a>

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve(3), 1000);
});

promise
  .then(result => {
    console.log(result); // 3
    return result * 2; // 6 - не выведет, просто вернет
  })
  .then(result => {
    console.log(result); // 6 в консоль
    return new Promise(resolve => setTimeout(() => resolve(result * 3), 1000)); // через секунду вернет 18, но не выведет
  })
  .then(result => {
    console.log(result); // а здесь уже через секунду (промис выше) выведет 18
  });


```

## <a id="21">setTimeout</a>

```js
console.log('Start');
setTimeout(() => console.log('Timeout 1'), 0);
setTimeout(() => console.log('Timeout 2'), 0);
console.log('End');
```
не стек, очередь, первый пришел первый вышел
Start, End, Timeout 1, Timeout 2
console.log('Start') → выводится сразу
setTimeout(..., 0) ставит Timeout 1 в очередь макрозадач, которая будет выполнена после завершения основного кода.
setTimeout(..., 0) ставит Timeout 2 также в очередь макрозадач (сразу после предыдущего).

console.log('End') → выводится сразу

Почему setTimeout(..., 0) не срабатывает сразу?
Потому что даже при 0 миллисекундах, setTimeout откладывает выполнение в отдельную очередь, которая выполняется после завершения текущего стека вызовов.

## <a id="22">obj a b</a>

```js
const obj = {
  a: 1,
  b: function() {
    return this.a;
  }
};
const b = obj.b; // Здесь функция b сохраняется отдельно от объекта, и теряет привязку к obj. Вызов b() — это обычный вызов функции, а не метод объекта.
console.log(b());
```

Объект obj содержит:

свойство a = 1,

метод b, который возвращает this.a.

Далее происходит присваивание:

В обычном вызове this указывает на:

undefined в строгом режиме ('use strict'),

глобальный объект (window в браузере, global в Node.js) — в нестрогом режиме.

Так как в глобальном объекте скорее всего нет свойства a, то результат: undefined

## <a id="23">Set</a>
```js
const set = new Set([1, 1, 2, 3, 4]);
console.log(set);
```

Выведет только уникальные значения, сет хранит уникальные значения

## <a id="24">objects compare</a>
```js
const a = {};
const b = {};
console.log(a == b);
console.log(a === b);
```

в обоих случаях false т.к. два объекта никогда не равны