1. Что такое Next.js и в чем его основные преимущества?

Next.js — это фреймворк поверх React, предоставляющий:

- серверный рендеринг (SSR),
- генерацию на этапе сборки (SSG),
- маршрутизацию из коробки,
- поддержку API-роутов,
- удобную настройку SEO и производительности.

2. Как работает маршрутизация в Next.js?

Next.js использует файловую маршрутизацию: структура в папке /pages определяет маршруты приложения. Например:

pages/index.tsx → /

pages/about.tsx → /about

Также поддерживается динамическая маршрутизация с помощью `[param].tsx`.

3. Что такое getServerSideProps и когда его использовать?

getServerSideProps вызывается на сервере при каждом запросе. Используется, когда данные нужно получать в реальном времени, например — авторизация, динамический контент.

```js
export async function getServerSideProps() {
  const res = await fetch(...);
  return { props: { data: await res.json() } };
}

```

4. Что такое getStaticProps и getStaticPaths?

getStaticProps — для предварительной генерации страниц на этапе сборки (SSG).

getStaticPaths — используется вместе с getStaticProps для генерации динамических страниц на этапе сборки.

5. В чем разница между SSR, SSG и ISR?

SSR (getServerSideProps) — страница генерируется на каждый запрос.

SSG (getStaticProps) — страница генерируется один раз на этапе сборки.

ISR (Incremental Static Regeneration) — гибрид: страница предварительно генерируется, а затем обновляется по таймеру revalidate.

6. Что такое App Router и чем он отличается от Pages Router?

App Router (папка /app) — новый способ маршрутизации с вложенными маршрутизируемыми layout'ами, React Server Components, async/await и параллельными маршрутами.

Pages Router (папка /pages) — классический способ маршрутизации.

7. Как добавить мета-теги и SEO в Next.js?
Ответ:

В Pages Router — через next/head:

```tsx
import Head from "next/head";
<Head><title>Title</title></Head>
```
В App Router — через generateMetadata и компонент Metadata.

8. Как реализовать API в Next.js?

В Pages Router можно создать файлы в pages/api. Эти файлы работают как serverless-функции:

```ts
// pages/api/hello.ts
export default function handler(req, res) {
  res.status(200).json({ message: "Hello" });
}
```

9. Как добавить middleware в Next.js?

Создать файл middleware.ts в корне проекта. Он выполняется до загрузки страницы и может использоваться для редиректов, аутентификации и т.д.

10. Как подключить глобальные стили и CSS-модули?
Ответ:

Глобальные стили импортируются в pages/_app.tsx или app/layout.tsx:

```ts
import "../styles/globals.css";
```
Локальные — через CSS Modules: Component.module.css, импортируются внутрь компонента.



