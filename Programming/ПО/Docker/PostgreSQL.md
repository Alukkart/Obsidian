```zsh title="terminal"
docker run --name postgres -e POSTGRES_PASSWORD=root -e POSTGRES_USER=user -p 5432:5432 -d postgres
```