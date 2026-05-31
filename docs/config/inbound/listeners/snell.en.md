# Snell

```{.yaml linenums="1"}
listeners:
- name: snell-in-1
  type: snell
  port: 10815 # Supports ports format, e.g., 200,302 or 200,204,401-429,501-503
  listen: 0.0.0.0
  psk: your-password
  version: 4 # Only supports 4/5
  udp: true # UDP over TCP tunnel, default is true
  # obfs-opts:
  #   mode: http # Optional: http / tls
  #   host: bing.com
  # rule: sub-rule-name1 # Uses global rules by default; if the sub-rule is not found, it falls back to global rules directly
  # proxy: proxy # If not empty, the inbound traffic will be handed over to the specified proxy directly (when not empty, the proxy name must be valid, otherwise an error will occur)

```
