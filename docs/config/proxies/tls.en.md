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
