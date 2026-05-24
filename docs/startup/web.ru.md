# Веб-интерфейсы: Mihomo API, Yacd-meta, metacubexd, XKeen-UI, Zashboard

Mihomo поддерживает несколько веб-интерфейсов для управления через REST API. Панели — это статический фронтенд, который отправляет запросы к HTTP API ядра.

## REST API (Controller)

Mihomo поднимает HTTP API на адресе, заданном в `external-controller`.

| Параметр конфига | Назначение |
|--------------------|------------|
| `external-controller` | `IP:порт` API (на роутере часто `0.0.0.0:9090` для доступа из LAN). |
| `secret` | Токен в заголовке `Authorization: Bearer …` (обязательно задать при доступе не только с localhost). |
| `external-ui` | Путь к **распакованной** папке UI на диске рядом с `-d`. |
| `external-ui-url` | URL **zip**-архива с UI — ядро может скачать/обновить. |
| `external-ui-name` | Подкаталог URL вида `http://…:9090/ui/<name>`. |
| `allow-lan` | Должен быть `true`, если заходите к API/UI с других машин. |

Типичный адрес после настройки: `http://<IP-роутера>:9090/ui/` (или с `external-ui-name`).

!!! warning "Безопасность"
    Не оставляйте `secret: ""` и `0.0.0.0:9090` без файрвола — любой в LAN сможет менять маршрутизацию и читать статистику.

### Пример конфигурации

```{.yaml linenums="1"}
external-controller: 0.0.0.0:9090
secret: "ваш_пароль"
external-ui: /opt/etc/mihomo/ui
external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
```

## Официальные панели

| Панель | Ссылка | Описание |
|--------|--------|----------|
| [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta) | [http://yacd.metacubex.one](http://yacd.metacubex.one) | Форк Yet Another Clash Dashboard под Mihomo: лёгкая панель, привычный интерфейс «прокси / правила / лог». |
| [Metacubexd](https://github.com/MetaCubeX/metacubexd) | [http://d.metacubex.one](http://d.metacubex.one) | «Домашняя» панель MetaCubeX, активно развивается, часто используется как дефолтный `external-ui-url`. Удобно сочетать с `profile.store-selected` — выбор узлов сохраняется между перезапусками. |
| [Zashboard](http://board.zash.run.place) | [http://board.zash.run.place](http://board.zash.run.place) | Лёгкая панель, устанавливается аналогично. |

## XKeen-UI

Веб-интерфейс для установщика [XKeen](https://github.com/Corvus-Malus/XKeen) на роутерах Keenetic. Это не встроенная панель бинарника Mihomo: XKeen-UI помогает настраивать окружение XKeen (в т.ч. Mihomo, nfqws, Xray).

Интегрируется через файловую систему роутера и автоматически обнаруживает API Mihomo. Если UI предлагает ввести URL или адрес API Mihomo, укажите тот же хост/порт, что в `external-controller`, и тот же `secret`.

## sign-craze

Отдельная Go-утилита для Keenetic + Entware: управление iptables/ipset, установка и переключение ядер (sing-box, xray, mihomo), опционально nfqws2 для DPI. По README проекта при включённом Web UI слушаются порты:

| Порт | Назначение |
|------|------------|
| 9090 | Zashboard (дашборд) |
| 9091 | admin API |
| 9092 | Routing Editor |

!!! warning "Конфликт портов с Mihomo"
    У Mihomo типичный `external-controller` — тоже HTTP API, часто на `0.0.0.0:9090`. Если на одном хосте работают sign-craze и Mihomo с одним портом, один из сервисов не поднимется. Смените `external-controller` в `config.yaml` Mihomo на другой порт (например, 9093) или следуйте рекомендациям sign-craze.

## Сравнение

| Проект | К чему подключается | Типичный сценарий |
|--------|---------------------|-------------------|
| XKeen-UI | XKeen на Keenetic | Установка, сценарии, связка с Mihomo/nfqws |
| Yacd-meta | REST Mihomo | Универсальная лёгкая панель |
| Metacubexd | REST Mihomo | Рекомендуемая панель Meta, близка к актуальным фичам ядра |
| sign-craze | sing-box / xray / mihomo + свой UI | Файрвол, DPI (nfqws2), переключение ядра |
| Zashboard | REST Mihomo | Лёгкая альтернатива |

## Доступ из локальной сети

Для доступа к веб-панели с других устройств (ПК, смартфон) укажите `external-controller: 0.0.0.0:9090` и настройте `secret`.

Подробнее: [Общие настройки](../config/general.md#external-control-api).
