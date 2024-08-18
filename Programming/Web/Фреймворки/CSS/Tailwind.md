---
tags:
  - nextjs
  - nodejs
  - react
---
# Tailwind - прогрессивный css framework
Очень сильный инструмент, особенно внутри node.js проекта, хорошая гибкость, множество настроек, совместим с React, предустановлен в Next.js, из минусов платные готовые компоненты в Официальной документации, но тут нам поможет [[shadcnUI]] или [[daisyUI]].
# Get started
#### Установи Tailwind CSS
```sh
> npm install -D tailwindcss # установка пакета
> npx tailwindcss init # создание файла tailwind.config.js
```

#### Добавь дериктивы Tailwind в твой CSS
```css
// ~style.css

@tailwind base;
@tailwind components;
@tailwind utilities;
```