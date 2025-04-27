# NTP

```{.yaml linenums="1"}
ntp:
  enable: true
  write-to-system: true
  server: time.apple.com
  port: 123
  interval: 30
```

## enable

Включение службы NTP

## write-to-system

Синхронизировать со временем системы, требуется запуск от root или с правами администратора.

## server

Адрес сервера NTP, по умолчанию `time.apple.com`

## port

Порт сервера NTP, по умолчанию `123`

## interval

Интервал синхронизации времени в минутах, по умолчанию 30 минут 