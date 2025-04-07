# Вопросы по ts
1. Чем TypeScript отличается от JavaScript?
Ответ:
TypeScript — это надмножество JavaScript, добавляющее статическую типизацию, интерфейсы, дженерики и другие возможности. Код на TypeScript компилируется в JavaScript, который может выполняться в любом браузере или Node.js.

2. Что такое интерфейс (interface) в TypeScript и зачем он нужен?
Ответ:
Интерфейс описывает структуру объекта. Он позволяет задать, какие свойства и методы должен иметь объект. Это помогает с автодополнением, валидацией типов и делает код более читаемым.

```ts
interface User {
  name: string;
  age: number;
}
const user: User = { name: "Alice", age: 30 };

```

3. Что такое unknown и чем оно отличается от any?
Ответ:

`any` отключает проверку типов — можно делать с переменной всё что угодно.

`unknown` — безопасная альтернатива: перед использованием нужно уточнить тип.

```ts
let x: unknown = "hello";
x.toUpperCase(); // Ошибка: сначала нужно проверить тип

```

4. Что такое enum и как его использовать?
Ответ:
enum (перечисление) — это способ задать набор именованных констант.

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
let dir: Direction = Direction.Up;

```

5. Что такое дженерики (generics)?
Ответ:
Дженерики позволяют писать обобщённый, типобезопасный код, который может работать с разными типами данных.

```ts
function identity<T>(arg: T): T {
  return arg;
}
identity<string>("Hello");

```

6. Что такое type и чем он отличается от interface?
Ответ:
Оба задают форму данных. Основные отличия:

interface чаще используется для объектов и может расширяться (extend).

type — более гибок: можно использовать объединения (|), пересечения (&) и алиасы для примитивов.

7. Как задать опциональное свойство в объекте?
Ответ:
Добавив ? после имени свойства:

```ts
interface User {
  name: string;
  age?: number;
}

```

8. Что такое утилитные типы, например Partial, Pick, Omit?
Ответ:
TypeScript предоставляет утилиты для трансформации типов:

Partial<T> — делает все поля необязательными.

Pick<T, K> — выбирает указанные поля.

Omit<T, K> — исключает указанные поля.

```ts
type User = { name: string; age: number };
type PartialUser = Partial<User>;
type NameOnly = Pick<User, "name">;

```

9. Как работает type narrowing (сужение типа)?
Ответ:
TypeScript может автоматически уточнять тип переменной в зависимости от условий (typeof, instanceof, проверок in, и т.д.)

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}

```

10. Можно ли использовать TypeScript вместе с React? Как?
Ответ:
Да. TypeScript отлично сочетается с React, обеспечивая типизацию props, state, useState, useEffect, и т.д.

```ts
type Props = { title: string };
const Header: React.FC<Props> = ({ title }) => <h1>{title}</h1>;

```