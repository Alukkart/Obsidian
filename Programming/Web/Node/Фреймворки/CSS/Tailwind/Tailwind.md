---
tags:
  - nextjs
  - nodejs
  - react
---
# Tailwind - прогрессивный CSS framework
Очень сильный инструмент, особенно внутри node.js проекта, хорошая гибкость, множество настроек, совместим с React, предустановлен в Next.js, из минусов платные готовые компоненты в Официальной документации, но тут нам поможет [[shadcnUI]] или [[daisyUI]].

# Сравнение кода компонента на ванильном CSS и Tailwind
```html
<div class="chat-notification"> <div class="chat-notification-logo-wrapper"> <img class="chat-notification-logo" src="/img/logo.svg" alt="ChitChat Logo"> </div> <div class="chat-notification-content"> <h4 class="chat-notification-title">ChitChat</h4> <p class="chat-notification-message">You have a new message!</p> </div> </div> <style> .chat-notification { display: flex; align-items: center; max-width: 24rem; margin: 0 auto; padding: 1.5rem; border-radius: 0.5rem; background-color: #fff; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); } .chat-notification-logo-wrapper { flex-shrink: 0; } .chat-notification-logo { height: 3rem; width: 3rem; } .chat-notification-content { margin-left: 1.5rem; } .chat-notification-title { color: #1a202c; font-size: 1.25rem; line-height: 1.25; } .chat-notification-message { color: #718096; font-size: 1rem; line-height: 1.5; } </style>
```

С Tailwind вы стилизуете элементы, применяя ранее существовавшие классы непосредственно в вашем HTML.

```html
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space-x-4"> <div class="shrink-0"> <img class="size-12" src="/img/logo.svg" alt="ChitChat Logo"> </div> <div> <div class="text-xl font-medium text-black">ChitChat</div> <p class="text-slate-500">You have a new message!</p> </div> </div>
```

Этот подход позволяет нам реализовать полностью пользовательский дизайн компонентов без написания одной строки пользовательского CSS.

Теперь я знаю, о чем ты думаешь _“это злодеяние, какой ужасный беспорядок!”_ и ты прав, это как-то уродливо. На самом деле почти невозможно думать, что это хорошая идея, когда вы впервые видите ее — **вы должны действительно попробовать это**.

Но как только вы действительно построите что-то таким образом, вы быстро заметите некоторые действительно важные преимущества:

- **Ты не тратишь энергию на придумывание названий классов**. Больше не добавляйте глупые названия классов, как `sidebar-inner-wrapper` просто чтобы иметь возможность что-то стилизовать, и больше не мучиться над идеальным абстрактным названием для чего-то, что на самом деле просто гибкий контейнер.
- **Ваш CSS перестает расти**. Используя традиционный подход, ваши файлы CSS становятся больше каждый раз, когда вы добавляете новую функцию. С утилитами все можно использовать повторно, поэтому вам редко нужно писать новый CSS.
- **Внесение изменений кажется более безопасным**. CSS является глобальным, и вы никогда не знаете, что вы ломаете, когда делаете изменения. Классы в вашем HTML являются локальными, поэтому вы можете изменить их, не беспокоясь о том, что что-то еще сломается.

Когда вы поймете, насколько продуктивно вы можете работать исключительно в HTML с предопределенными классами полезностей, работа любым другим способом будет казаться пыткой.

## Почему бы просто не использовать встроенные стили?

Общая реакция на этот подход вызывает удивление, “разве это не просто встроенные стили?” и в некотором роде это — вы применяете стили непосредственно к элементам вместо того, чтобы присваивать им название класса, а затем стилизовать этот класс.

Но использование классов утилиты имеет несколько важных преимуществ перед встроенными стилями:
- **Проектирование с ограничениями**. Используя встроенные стили, каждое значение является магическим числом. С помощью утилит вы выбираете стили из предопределенных [система проектирования](https://tailwindcss.com/docs/theme), что значительно упрощает построение визуально согласованных UI.
- **[[Адаптивный дизайн]]**. Вы не можете использовать медиа-запросы в встроенных стилях, но можете использовать Tailwind’s [адаптивные утилиты](https://tailwindcss.com/docs/responsive-design) легко создавать полностью адаптивные интерфейсы.
- **Hover, active и др. Внутренние стили не могут быть нацелены на такие состояния, как зависание или фокусировка, но Tailwind’s [варианты состояния](https://tailwindcss.com/docs/hover-focus-and-other-states) упростите стилизацию этих состояний с помощью классов полезности.
# Get started
#### Установи Tailwind CSS
```zsh title="terminal"
npm install -D tailwindcss # установка пакета
npx tailwindcss init # создание файла tailwind.config.js
```

#### Добавь директивы Tailwind в твой CSS
```css
// ~style.css

@tailwind base;
@tailwind components;
@tailwind utilities;
```

