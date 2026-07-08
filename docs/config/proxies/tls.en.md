# TLS Configuration

```{.yaml linenums="1"}
proxies:
- name: "tls-example"
  tls: true
  sni: example.com
  servername: example.com
  fingerprint: xxx
  alpn:
  - h2
  - http/1.1
  skip-cert-verify: true
  # certificate: xxxx
  # private-key: xxx
  client-fingerprint: chrome
  reality-opts:
    public-key: xxxx
    short-id: xxxx
    support-x25519mlkem768: true
  ech-opts:
    enable: true
    config: base64_encoded_config
    # query-server-name: xxx.com
  tlsmirror-opts:
    primary-key: MDEyMzQ1Njc4OWFiY2RlZjAxMjM0NTY3ODlhYmNkZWY=
    explicit-nonce-ciphersuites: [
      156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171,
      172, 173, 49195, 49196, 49197, 49198, 49199, 49200, 49201, 49202, 49290, 49291,
      49293, 49316, 49317, 49318, 49319, 49320, 49321, 49322, 49323, 49324, 49325,
      49326, 49327, 52392, 52393, 52394, 52395, 52396, 52397, 52398
    ]
    defer-instance-derived-write-time:
      base-nanoseconds: 0
      uniform-random-multiplier-nanoseconds: 0
    transport-layer-padding:
      enabled: false
    connection-enrolment:
      primary-egress-outbound: ""
    sequence-watermarking-enabled: false
    embedded-traffic-generator:
      steps:
        - name: example
          host: example.com
          path: /
          method: GET
          headers:
            - name: User-Agent
              value: mihomo
            - name: Accept
              values:
                - text/html
                - application/xhtml+xml
          connection-ready: true
          connection-recall-exit: true
          h2-do-not-wait-for-download-finish: false
          wait-time:
            base-nanoseconds: 1000000000
            uniform-random-multiplier-nanoseconds: 0
          next-step:
            - weight: 1
              goto-location: 0
```

## tls

Enables TLS, applicable only to protocols that use `tls`, with the `trojan` protocol requiring it to be enabled.

## sni/servername

The server name indication, referred to as `servername` in [`VMess`](./vmess.md)/[`VLESS`](./vless.md). If left empty, it defaults to the address in `server`.

## fingerprint

Certificate fingerprint, applicable only to protocols that use `tls`. You can obtain the fingerprint using the following command:
```bash
openssl x509 -noout -fingerprint -sha256 -inform pem -in yourcert.pem
```
Alternatively, you can obtain it through the "Certificates" section of the "SHA256 Fingerprint" in Chrome's "Certificate Viewer".

!!! warning

    * When a leaf certificate (i.e., a certificate containing an SNI name) is entered, only the fingerprint of the certificate sent by the server is verified; no additional checks are performed.

    * When the fingerprint of other types of certificates (such as intermediate or root certificates) is entered, it will be verified whether the certificate chain sent by the server was issued by that certificate. From v1.19.20 onwards, the SNI/servername requirement must also be met.

    * The fingerprint in this field is the fingerprint of the complete certificate, not the "certificate public key fingerprint" defined in HPKP. Please do not confuse them.

## alpn

List of supported Application Layer Protocol Negotiation options, arranged in order of priority.

If both peers support ALPN, the selected protocol will be one from this list; if there are no mutually supported protocols, the connection will fail.

Refer to [Application-Layer Protocol Negotiation](https://en.wikipedia.org/wiki/Application-Layer_Protocol_Negotiation)

## skip-cert-verify

Bypasses certificate verification, applicable only to protocols that use `tls`.

## certificate

If filled, this enables [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) (must be filled in with private-key). The content is the certificate in PEM format or the path to the certificate.

## private-key

If filled, this enables [mTLS](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/) (must be filled in with certificate). The content is the private key corresponding to the certificate in PEM format or the path to the private key.

## client-fingerprint

Client uTLS fingerprint, applicable only to [`VMess`](./vmess.md)/[`VLESS`](./vless.md)/[`Trojan`](./trojan.md)/[`AnyTLS`](./anytls.md) protocols.

!!! note
    Options: `chrome`, `firefox`, `safari`, `iOS`, `android`, `edge`, `360`, `qq`, `random`
    If `random` is selected, a modern browser fingerprint will be generated based on Cloudflare Radar data.

## reality-opts

Configuration for reality; if not empty, reality will be enabled.

### reality-opts.public-key

Public key corresponding to the reality server's private key.

### reality-opts.short-id

One of the server's short IDs.

### reality-opts.support-x25519mlkem768

Support for X25519-MLKEM768 key exchange.

## ech-opts

### ech-opts.enable

Enables ECH (Encrypted Client Hello). Setting it to true enables ECH.

### ech-opts.config

The ECH configuration, if empty, will be resolved via DNS; otherwise, it will be specified by this value, in the format of base64 encoded ech parameters (For example, `AEn+DQBFKwAgACABWIHUGj4u+PIggYXcR5JF0gYk3dCRioBW8uJq9H4mKAAIAAEAAQABAANAEnB1YmxpYy50bHMtZWNoLmRldgAA`).

!!! info
    You can use the command `mihomo generate ech-keypair test.com` to generate a compliant self-signed ECH configuration pair for both the server and client. Please replace `test.com` with the SNI domain name you want to expose. The content after `Config:` in the output can be filled here, and the content after `Key:` should be filled in the server-side ECH configuration (`ech-key` in mihomo's listeners).

### ech-opts.query-server-name

Optional, if not empty, it is used to specify the domain name when resolving via DNS.

## tlsmirror-opts

When `tls` is `true`, configuring `tlsmirror-opts` enables tlsmirror. The TLS carrier used by tlsmirror uses `servername`, `alpn`, `skip-cert-verify`, `fingerprint`, `certificate`, `private-key`, `client-fingerprint`, and `ech-opts` from the same outbound. If `servername` is empty, `server` is used.

!!! note
    Currently, only VMess supports enabling tlsmirror. Do not use it with other protocols.

### tlsmirror-opts.primary-key

Required, base64 encoding of the 32-byte primary key.

### tlsmirror-opts.explicit-nonce-ciphersuites

Cipher suites with explicit nonce used by the TLS 1.2 carrier.

### tlsmirror-opts.defer-instance-derived-write-time

Delay before the first write.

### tlsmirror-opts.transport-layer-padding

Transport-layer padding settings.

### tlsmirror-opts.connection-enrolment

v2ray-compatible connection enrolment settings. For mihomo VMess outbound, `primary-egress-outbound` is usually left empty.

#### tlsmirror-opts.connection-enrolment.primary-ingress-outbound

Inbound-side control outbound tag used for connection enrolment. Usually used to match v2ray-compatible server configuration.

#### tlsmirror-opts.connection-enrolment.primary-egress-outbound

Outbound-side control outbound tag used for connection enrolment. For mihomo VMess outbound, this is usually left empty; v2ray can set a dedicated control outbound tag.

### tlsmirror-opts.sequence-watermarking-enabled

Whether to enable sequence watermarking.

### tlsmirror-opts.embedded-traffic-generator

Generates extra HTTP carrier traffic. The protocol is determined by `alpn`. It is disabled when `steps` is not configured.

#### tlsmirror-opts.embedded-traffic-generator.steps

List of HTTP carrier traffic steps. Steps run in order unless `next-step` selects another step.

#### tlsmirror-opts.embedded-traffic-generator.steps.name

Step name, used only as an identifier.

#### tlsmirror-opts.embedded-traffic-generator.steps.host

HTTP request target host.

#### tlsmirror-opts.embedded-traffic-generator.steps.path

HTTP request path.

#### tlsmirror-opts.embedded-traffic-generator.steps.method

HTTP request method.

#### tlsmirror-opts.embedded-traffic-generator.steps.headers

HTTP request header list. Each item uses `name` for the header name, `value` for a single value, or `values` for multiple values.

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-ready

Deliver the proxy connection after this step finishes.

#### tlsmirror-opts.embedded-traffic-generator.steps.connection-recall-exit

Exit the carrier traffic after the proxy connection is closed.

#### tlsmirror-opts.embedded-traffic-generator.steps.h2-do-not-wait-for-download-finish

When the carrier protocol is `h2`, do not wait for the response body to finish reading.

#### tlsmirror-opts.embedded-traffic-generator.steps.wait-time

Wait time after the step finishes. Fields are the same as [tlsmirror-opts.defer-instance-derived-write-time](#tlsmirror-optsdefer-instance-derived-write-time).

#### tlsmirror-opts.embedded-traffic-generator.steps.next-step

Weighted candidate list for the next step. `weight` is the weight, and `goto-location` is the target step index.
