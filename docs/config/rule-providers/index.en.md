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
```

## name

Required, such as `google`, must be unique.

## type

Required, `provider` type, options are `http` / `file`.

## url

Must be configured if the type is `http`.

## path

Optional, file path, must be unique. If not provided, the MD5 of the URL will be used as the filename.

For security reasons, this path is restricted to only allow locations within `HomeDir` (configured with the -d startup parameter). If you want to store it in any location, set the environment variable `SKIP_SAFE_PATH_CHECK=1`.

## interval

The update interval for the `provider`, in seconds.

## proxy

Download/update through the specified proxy.

## behavior

Behavior, options are `domain` / `ipcidr` / `classical`, corresponding to different formats of rule-provider files. Please fill in according to the actual format.

## format

Format, options are `yaml` / `text` / `mrs`/`inline`, default is `yaml`.

Currently, `mrs` behavior only supports `domain` / `ipcidr`. You can convert using `mihomo convert-ruleset domain/ipcidr yaml/text XXX.yaml XXX.mrs`.

## size-limit

The maximum size of downloadable files is restricted, with the default being 0, which means no size limit; the unit is bytes (`b`)

## payload

Content, only effective when `format` is `inline`
