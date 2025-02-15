## Базовое создание контейнера и бд
```zsh title="terminal"
docker run --name postgres -e POSTGRES_PASSWORD=root -e POSTGRES_USER=user -p 5432:5432 -d postgres
```

## Восстановление бд из дампа
```zsh title="terminal"
pg_restore -h localhost -U a_user -d a_db -p 5432 -v file.dump
```
для восстановления дампа необходима **пустая бд без таблиц**

## Создание бд в существующем контейнере
Подключитесь к контейнеру
```zsh title="terminal"
docker exec -it <название контейнера> psql -U postgres

```

Затем в psql выполните:
```zsh title="terminal"
CREATE DATABASE mydatabase;
```
