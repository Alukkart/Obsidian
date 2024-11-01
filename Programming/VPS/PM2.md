# Установка
PM2 - это демон-менеджер процессов, который поможет вам управлять и поддерживать ваше приложение в режиме онлайн. Начать работу с PM2 очень просто, он предлагается в виде простого и интуитивно понятного CLI, устанавливаемого через NPM.
```zsh title="terminal"
npm install pm2@latest -g
# or
yarn global add pm2
```

# Запуск приложения
```zsh title="terminal"
#basic
pm2 start app.js

pm2 start bashscript.sh
pm2 start python-app.py --watch
pm2 start binary-file -- --port 1520
```

## Параметры запуска
```
# Specify an app name
--name <app_name>

# Watch and Restart app when files change
--watch

# Set memory threshold for app reload
--max-memory-restart <200MB>

# Specify log file
--log <log_path>

# Pass extra arguments to the script
-- arg1 arg2 arg3

# Delay between automatic restarts
--restart-delay <delay in ms>

# Prefix logs with time
--time

# Do not auto restart app
--no-autorestart

# Specify cron for forced restart
--cron <cron_pattern>

# Attach to application log
--no-daemon
```

# Управление процессами
```zsh title="terminal"
pm2 restart app_name
pm2 reload app_name
pm2 stop app_name
pm2 delete app_name
```

Вместо `app_name` вы можете передать:
- `all` что бы взаимодействовать со всеми процессами
- `id` что бы обратиться к процессу по id

# Проверка статуса, логов и метрик
## Вывод списка процессов
```zsh title="terminal"
pm2 [list|ls|status]
```

## Просмотр логов
```zsh title="terminal"
pm2 logs
```

