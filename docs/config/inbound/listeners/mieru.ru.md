# Mieru

```{.yaml linenums="1"}
listeners:
- name: mieru-in-1
  type: mieru
  port: 10818
  listen: 0.0.0.0
  # routing-mark: 0 # Устанавливает routing-mark для прослушивающего сокета (поддерживается только в Linux)
  transport: TCP # Поддерживает TCP или UDP
  users:
    username1: password1
    username2: password2
  # Строка base64, используемая для тонкой настройки поведения сети
  # traffic-pattern: ""
  # Если включено, и клиент не отправляет подсказку пользователя, прокси-сервер отклонит соединение
  # user-hint-is-mandatory: false
```

[Общие поля](./index.md)
