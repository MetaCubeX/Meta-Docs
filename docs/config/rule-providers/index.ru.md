# rule-provider

```{.yaml linenums="1"}
rule-providers:
  google:
    type: http
    path: ./rule1.yaml
    url: "https://raw.githubusercontent.com/../Google.yaml"
    interval: 600
    proxy: DIRECT
    behavior: classical
    format: yaml
    size-limit: 0
    payload:
      - 'DOMAIN-SUFFIX,google.com'
```

## name

Обязательное поле, например `google`, должно быть уникальным.

## type

Обязательное поле, тип `provider`, возможные значения: `http` / `file` / `inline`.

## url

Должно быть настроено, если тип `http`.

## path

Необязательно, путь к файлу, должен быть уникальным. Если не указан, для имени файла будет использован MD5 от URL.

По соображениям безопасности этот путь ограничен и допускается только в пределах `HomeDir` (настраивается параметром запуска -d). Если вы хотите хранить его в любом месте, установите переменную окружения `SKIP_SAFE_PATH_CHECK=1`.

## interval

Интервал обновления для `provider`, в секундах.

## proxy

Загрузка/обновление через указанный прокси.

## behavior

Поведение, возможные значения: `domain` / `ipcidr` / `classical`, соответствуют различным форматам файлов rule-provider. Пожалуйста, заполните в соответствии с фактическим форматом.

## format

Формат, возможные значения: `yaml` / `text` / `mrs`, по умолчанию `yaml`.

В настоящее время `mrs` поддерживает только поведение `domain` / `ipcidr`. Вы можете конвертировать, используя `mihomo convert-ruleset domain/ipcidr yaml/text XXX.yaml XXX.mrs`.

## size-limit

Ограничение максимального размера загружаемых файлов, по умолчанию 0, что означает отсутствие ограничения размера; единица измерения - байты (`b`)

## payload

Содержимое, действует только когда `type` имеет значение `inline` 