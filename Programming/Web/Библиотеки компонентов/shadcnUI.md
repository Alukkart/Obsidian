# [shadcnUI](https://ui.shadcn.com/) Библиотека компонентов для [[Tailwind]]
больше всего подходит в тех случаях когда не нужен "особенный" дизайн
# Плюсы:
1. легко совместить с React ивентами
2. качественные готовые компоненты
3. глубокая кастомизация
4. ассортимент компонентов
5. отличная совместимость с next.js
# Минусы:
1. требует много знаний
2. не проста в использовании
# Установка с next.js
## Создание проекта
Начнем с создания проекта с помощью `create-next-app`
```zsh title="terminal"
npx create-next-app@latest my-app --typescript --tailwind --eslint
```

## Запуск [CLI (command-line interface)](https://en.wikipedia.org/wiki/Command-line_interface)
Запустите init команду для настройки вашего проекта:
```zsh title="terminal"
npx shadcn-ui@latest init
```
## Настройка `components.json`
Вам будет предложено несколько вопросов для настройки `components.json`:
```zsh title="terminal"
>Which style would you like to use? › Default
>Which color would you like to use as base color? › Slate
>Do you want to use CSS variables for colors? › no / yes
```
## Шрифты
### Импорт шрифта в RootLayout
```ts showLineNumbers {3, 7-10, 17-18}
// app/layout.tsx
import "@/styles/globals.css" 
import { Inter as FontSans } from "next/font/google"

import { cn } from "@/lib/utils"

const fontSans = FontSans({
  subsets: ["latin"],
  variable: "--font-sans",
})

export default function RootLayout({ children }: RootLayoutProps) {
  return (
    <html lang="en" suppressHydrationWarning>
      <head />
      <body
        className={cn(
          "min-h-screen bg-background font-sans antialiased",
          fontSans.variable
        )}
      >
        ...
      </body>
    </html>
  )
}

```
### Настройка `theme.extend.fontFamily` в `tailwind.config.ts`
```ts showLineNumbers {9-12}
// tailwind.config.ts
const { fontFamily } = require("tailwindcss/defaultTheme")

/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ["class"],
  content: ["app/**/*.{ts,tsx}", "components/**/*.{ts,tsx}"],
  theme: {
    extend: {
      fontFamily: {
        sans: ["var(--font-sans)", ...fontFamily.sans],
      },
    },
  },
}

```
## Структура проекта

## Добавление компонентов
Теперь вы можете начать добавлять компоненты в свой проект.
```zsh title="terminal"
npx shadcn-ui@latest add button
```
Команда выше добавит `Button` компонент в ваш проект. Затем вы можете импортировать его следующим образом:
```tsx showLineNumbers {1, 7}
// shared/components/ui/button
import { Button } from "@shared/components/ui/button"

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```