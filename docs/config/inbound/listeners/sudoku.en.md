# Sudoku

```{.yaml linenums="1"}
listeners:
- name: sudoku-in-1
  type: sudoku
  port: 8443 # Only a single port is supported
  listen: 0.0.0.0
  # routing-mark: 0 # Sets the routing-mark for the listening socket (Linux only)
  key: "<server_key>" # Public key from a Sudoku-generated ED25519 key pair, or any UUID used as a shared key
  aead-method: chacha20-poly1305 # chacha20-poly1305, aes-128-gcm, or none (not recommended; none provides no AEAD protection)
  padding-min: 1 # Minimum padding ratio (0-100)
  padding-max: 15 # Maximum padding ratio (0-100, must be >= padding-min)
  table-type: prefer_ascii # prefer_ascii, prefer_entropy, up_ascii_down_entropy, or up_entropy_down_ascii
  # custom-table: xpxvvpvv # Custom byte layout containing 2 x, 2 p, and 4 v characters; applies only to the entropy direction
  # custom-tables: ["xpxvvpvv", "vxpvxvvp"] # List of byte layouts for rotation; overrides custom-table when non-empty
  handshake-timeout: 5 # Optional, in seconds
  enable-pure-downlink: false # false uses bandwidth-optimized downlink; true uses pure Sudoku downlink
  httpmask:
    disable: false # true disables all HTTP masking and tunneling
    mode: legacy # legacy (default), stream, poll, auto, or ws
    # path-root: "" # First-level HTTP tunnel path prefix; both sides must match
  # Optionally pass rejected HTTPMask traffic to another service sharing the same port:
  # fallback: "127.0.0.1:80"
  # Legacy-compatible forms of the httpmask fields:
  # disable-http-mask: false
  # http-mask-mode: legacy
  # path-root: ""
  # mux-option:
  #   padding: true
  #   brutal:
  #     enabled: true
  #     up: 1000 # Mbps by default
  #     down: 1000
```

[General Fields](./index.md)
