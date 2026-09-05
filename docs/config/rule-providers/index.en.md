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
    # path-in-bundle: "geo/geosite/cn.mrs"
    size-limit: 0
    header:
      User-Agent:
      - "mihomo/1.18.3"
      Authorization:
      - 'token 1231231'
    payload:
      - 'DOMAIN-SUFFIX,google.com'
```

## name

Required, such as `google`, must be unique.

## type

Required, `provider` type, options are `http` / `file` / `inline`.

## url

Must be configured if the type is `http`.

## path

Optional, file path, must be unique. If not provided, the MD5 of the URL will be used as the filename.

For security reasons, this path is restricted to only allow locations within `HomeDir` (configured with the -d startup parameter). If you want to store it in other locations, please specify additional safe paths by setting the `SAFE_PATHS` environment variable. The syntax of this environment variable is the same as the PATH environment variable parsing rules of this operating system (that is, it is separated by semicolons in Windows and by colons in other systems).

## interval

The update interval for the `provider`, in seconds.

## proxy

Download/update through the specified proxy.

## behavior

Behavior, options are `domain` / `ipcidr` / `classical`, corresponding to different formats of rule-provider files. Please fill in according to the actual format.

## format

Format, options are `yaml` / `text` / `mrs`, default is `yaml`.

Currently, `mrs` behavior only supports `domain` / `ipcidr`. You can convert using `mihomo convert-ruleset domain/ipcidr yaml/text XXX.yaml XXX.mrs`.

## path-in-bundle

When this option is set, if the local file does not exist, it will be extracted from BundleMRS.7z in the Home Dir first. The value of this option specifies the path in BundleMRS.7z.

BundleMRS.7z can be downloaded from https://github.com/MetaCubeX/meta-rules-dat

## size-limit

The maximum size of downloadable files is restricted, with the default being 0, which means no size limit; the unit is bytes (`b`)

## header

Custom HTTP request headers.

## payload

Content, only effective when `type` is `inline`
