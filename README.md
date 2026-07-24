# JSON → Proxy URL API

> Конвертирует JSON-конфиги (Xray / Sing-box) в proxy-ссылки.

**Base URL:** `https://p.kfwl.lol`

---

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/json` | Конвертация JSON → proxy URL |
| `GET` | `/?=url/<sub>` | Существующий (без изменений) |
| `GET` | `/vpn/<name>/<token>` | Существующий (без изменений) |

---

## POST `/json`

### Headers

```
Content-Type: application/json
Accept: text/plain | application/json
```

### Request Body

Любой валидный JSON из перечисленных ниже форматов:

| Формат | Описание |
|--------|----------|
| Xray full-config | `dict` с полной конфигурацией (вкл. outbounds) |
| Sing-box full-config | `dict` с полной конфигурацией |
| Один outbound | Xray (`protocol`) или Sing-box (`type`) |
| Массив outbounds | Смешивать форматы можно |

### Response

**text/plain** (по умолчанию) — по одной ссылке на строку:

```
vless://uuid@host:443?...
hysteria2://pw@host:443?...
```

**application/json** — объект:

```json
{
  "count": 2,
  "links": [
    "vless://uuid@host:443?...",
    "hysteria2://pw@host:443?..."
  ]
}
```

---

## Supported Protocols

| Протокол | Параметры |
|----------|-----------|
| `vless` | id, encryption, flow |
| `vmess` | id, alterId, security |
| `trojan` | password |
| `shadowsocks` | method, password |
| `hysteria2` | password, obfs |
| `tuic` | password, uuid |
| `anytls` | password |

### Transports

`tcp`, `ws`, `httpupgrade`, `grpc`, `http/h2`, `xhttp` — с полным `extra`.

---

## Examples

### Xray — VLESS over TCP+TLS

```bash
curl -X POST https://p.kfwl.lol/json \
  -H 'Content-Type: application/json' \
  -d '{
    "protocol": "vless",
    "tag": "test",
    "settings": {
      "vnext": [{
        "address": "1.2.3.4",
        "port": 443,
        "users": [{
          "id": "UUID",
          "encryption": "none",
          "flow": ""
        }]
      }]
    },
    "streamSettings": {
      "network": "tcp",
      "security": "tls",
      "tlsSettings": {
        "serverName": "test.com"
      }
    }
  }'
```

### Sing-box — Hysteria2

```bash
curl -X POST https://p.kfwl.lol/json \
  -H 'Content-Type: application/json' \
  -d '{
    "type": "hysteria2",
    "tag": "FR",
    "server": "fr.example.com",
    "server_port": 443,
    "password": "pw",
    "obfs": {
      "type": "salamander",
      "password": "salt"
    },
    "tls": {
      "enabled": true,
      "server_name": "fr.example.com",
      "alpn": ["h3"]
    }
  }'
```

### Batch — Array (JSON response)

```bash
curl -X POST https://p.kfwl.lol/json \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  --data-binary @configs.json
```

**`configs.json`:**

```json
[
  { "protocol": "vless", "settings": { "vnext": [...] } },
  { "type": "hysteria2", "server": "host.com", ... }
]
```

---

## Errors

| Code | Причина |
|------|---------|
| `400` | Невалидный JSON |
| `405` | Метод не POST |
| `422` | В теле нет поддерживаемых proxy |
| `500` | Внутренняя ошибка сервера |
