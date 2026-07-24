JSON to proxy URL API

POST xray или sing-box конфиг, получи vless/vmess/trojan/ss/hysteria2/tuic/anytls URLs.
Endpoint
POST /json
тело - любой валидный JSON (см. ниже)
Content-Type
application/json
Accept
text/plain (по умолч.) или application/json
Что можно прислать

    Xray full-config (dict с outbounds) или массив таких
    Sing-box full-config или массив таких
    Один outbound: xray (с protocol) или sing-box (с type)
    Массив outbounds (смешивать форматы можно)

Поддерживаемые протоколы

vless, vmess, trojan, shadowsocks, hysteria2, tuic, anytls. Транспорты: tcp, ws, httpupgrade, grpc, http/h2, xhttp (с полным extra).
Пример - xray outbound

curl -X POST https://p.kfwl.lol/json -H 'Content-Type: application/json' -d '{
  "protocol":"vless","tag":"test",
  "settings":{"vnext":[{"address":"1.2.3.4","port":443,
    "users":[{"id":"UUID","encryption":"none","flow":""}]}]},
  "streamSettings":{"network":"tcp","security":"tls",
    "tlsSettings":{"serverName":"test.com"}}}'

Пример - sing-box outbound

curl -X POST https://p.kfwl.lol/json -H 'Content-Type: application/json' -d '{
  "type":"hysteria2","tag":"FR","server":"fr.example.com","server_port":443,
  "password":"pw","obfs":{"type":"salamander","password":"salt"},
  "tls":{"enabled":true,"server_name":"fr.example.com","alpn":["h3"]}}'

Пример - массив, JSON response

curl -X POST https://p.kfwl.lol/json \
  -H 'Content-Type: application/json' -H 'Accept: application/json' \
  --data-binary @configs.json
# -> {"count":21,"links":["vless://...","hysteria2://...",...]}

Ошибки
400
невалидный JSON
405
метод не POST
422
в теле нет поддерживаемых proxy
500
внутренняя ошибка

Существующие endpoint'ы (/?=url/<sub>, /vpn/<name>/<token>) работают как раньше.
