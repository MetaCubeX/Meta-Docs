# Predefined Proxy Group

## GLOBAL

Default fill all proxy-groups and proxies

The strategy group can also be customized in the configuration file

However, the web panel and some client applications sort according to the order of policy groups within the GLOBAL policy group

It is recommended to write out all the policy groups of the current configuration in full (except for the GLOBAL itself) when customizing the GLOBAL policy group

If GLOBAL is not customized and nothing has been selected in it, GLOBAL selects its first member, `DIRECT`. Switching to `global` mode then sends all traffic directly. Once a policy is selected in GLOBAL, switching between `rule` and `global` mode keeps that selection.

To set GLOBAL's initial selection, customize GLOBAL and set [`default-selected`](./index.md#default-selected):

```{.yaml linenums="1"}
proxy-groups:
  - name: GLOBAL
    type: select
    proxies: [proxy-a, proxy-b, DIRECT]
    default-selected: proxy-a
```
