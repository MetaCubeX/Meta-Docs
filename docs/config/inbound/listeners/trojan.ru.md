# Trojan

```{.yaml linenums="1"}
listeners:
- name: trojan-in-1
  type: trojan
  port: 10819 # поддерживает формат ports, например 200,302 или 200,204,401-429,501-503
  listen: 0.0.0.0
  # rule: sub-rule-name1 # по умолчанию использует rules, если sub-rule не найдено, то напрямую использует rules
  # proxy: proxy # если не пусто, то входящий трафик напрямую передается указанному proxy (когда proxy не пусто, имя proxy должно быть корректным, иначе возникнет ошибка)
  users:
    - username: 1
      password: 9d0cb9d0-964f-4ef6-897d-6c6b3ccf9e68
  # ws-path: "/" # если не пусто, то включается транспортный уровень websocket
  # grpc-service-name: "GunService" # если не пусто, то включается транспортный уровень grpc
  # следующие два параметра, если заполнены, включают tls (необходимо заполнить оба)
  certificate: ./server.crt
  private-key: ./server.key
  # если заполнен reality-config, то включается reality (не может использоваться одновременно с certificate и private-key)
  # reality-config:
  #   dest: test.com:443
  #   private-key: jNXHt1yRo0vDuchQlIP6Z0ZvjT3KtzVI-T4E7RoLJS0 # можно сгенерировать командой mihomo generate reality-keypair
  #   short-id:
  #     - 0123456789abcdef
  #   server-names:
  #     - test.com
  # ss-option: # аналогично настройке `shadowsocks` в trojan-go
  #   enabled: false
  #   method: aes-128-gcm # aes-128-gcm/aes-256-gcm/chacha20-ietf-poly1305
  #   password: "example"
  ### Примечание: для trojan listener необходимо заполнить по крайней мере один из параметров: "certificate и private-key" или "reality-config" или "ss-option" ###
``` 