# Предварительная выборка данных

## Данные страницы верхнего уровня

Есть много способов предварительно получить данные для SWR. Для запросов верхнего уровня настоятельно рекомендуется использовать [`rel="preload"`](https://developer.mozilla.org/ru/docs/Web/HTML/Preloading_content):

```html
<link rel="preload" href="/api/data" as="fetch" crossorigin="anonymous">
```

Просто поместите его в свой HTML `<head>`. Это просто, быстро и нативно.

Он выполнит предварительную выборку данных при загрузке HTML, даже до того, как начнется загрузка JavaScript. Все ваши входящие запросы на выборку с одним и тем же URL-адресом будут повторно использовать этот результат (включая, конечно, SWR).

## Программная предварительная выборка

Иногда вы хотите предварительно загрузить ресурс условно. Например, предварительная загрузка данных, когда пользователь [наводит](https://github.com/GoogleChromeLabs/quicklink) [курсор](https://github.com/guess-js/guess) [на ссылку](https://instant.page). Самый интуитивно понятный способ — иметь функцию для повторной выборки и установки кеша с помощью глобального [mutate](/docs/mutation):

```js
import { mutate } from 'swr'

function prefetch () {
  mutate('/api/data', fetch('/api/data').then(res => res.json()))
  // вторым параметром является промис
  // SWR использует его результат, после того, как он разрешится
}
```

Вместе с такими техниками, как [предзагрузка страниц](https://nextjs.org/docs/api-reference/next/router#routerprefetch) в Next.js, вы сможете мгновенно загружать как следующую страницу, так и данные.

## Предварительное заполнение данных

Если вы хотите предварительно заполнить существующие данные в кэш SWR, вы можете использовать опцию `fallbackData`. Например:

```jsx
useSWR('/api/data', fetcher, { fallbackData: prefetchedData })
```

Если SWR ещё не получил данные, этот хук вернёт `prefetchedData` в качестве запасного варианта.

Вы также можете настроить это для всех SWR хуков и множественных ключей с помощью `<SWRConfig>` и опцией `fallback`. Смотрите подробности в разделе [Next.js SSG и SSR](/docs/with-nextjs).