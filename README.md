# vpnproxy

Self-hosted subscription proxy & converter для VPN-клиентов.

## Что делает

- Проксирует подписки (`/vpn/<name>/<token>`) с ретраями и таймаутами.
- Разбирает xray / sing-box / clash JSON и base64 подписки.
- Конвертит outbound-конфиги обратно в ссылки `vless://`, `vmess://`, `trojan://`, `ss://`, `hy2://`, `tuic://`, `anytls://`.
- Поддерживает транспорты: tcp, ws, grpc, http, xhttp, httpupgrade; security: tls, reality, none.
- REST API: `POST /json` — конвертер JSON → массив ссылок; `GET /docs` — HTML-документация.
- Форматированные страницы подписок (`/<name>`) с QR-кодами и списком нод.

## Статус

Репозиторий публичный и создан **исключительно для ознакомления**.
Исходный код здесь **не публикуется** — только описание проекта и лицензия.

## Стек

Python 3.11 · aiohttp · nginx · systemd · Debian 12.

## Демо

- `p.kfwl.lol/docs` — документация API.
- `p.kfwl.lol/json` — endpoint конвертера.

## Автор

[@кфвл](https://kfwlrus.t.me)
