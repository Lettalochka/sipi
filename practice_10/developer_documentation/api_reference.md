# API-документация разработчика

Документ описывает серверные API проекта `Legal Store`. API реализованы как
Next.js Route Handlers в каталоге `src/app/api`.

## Общие сведения

- Базовый URL локальной разработки: `http://localhost:3000`
- Формат обмена: JSON
- Аутентификация пользовательских методов: сессионная cookie NextAuth
- Защита служебных jobs-методов: заголовок `x-jobs-secret`
- Для части публичных методов используется rate limit по IP-адресу

## Сводная таблица API

| Метод | Endpoint | Назначение | Доступ |
| --- | --- | --- | --- |
| `GET/POST` | `/api/auth/[...nextauth]` | Служебные маршруты NextAuth | публичный/сессионный |
| `POST` | `/api/auth/register` | Регистрация пользователя | публичный |
| `POST` | `/api/checkout/create-payment` | Создание заказа и ссылки оплаты | публичный/сессионный |
| `POST` | `/api/digiseller/callback` | Webhook от Digiseller | Digiseller |
| `GET` | `/api/digiseller/return` | Возврат пользователя после оплаты | публичный |
| `GET` | `/api/favorites` | Получение избранного | авторизованный пользователь |
| `POST` | `/api/favorites/toggle` | Добавление/удаление товара из избранного | авторизованный пользователь |
| `POST` | `/api/favorites/sync` | Синхронизация локального избранного с аккаунтом | авторизованный пользователь |
| `POST` | `/api/jobs/fulfill` | Обработка ожидающих выдач товара | служебный |
| `POST` | `/api/jobs/sync-digiseller` | Синхронизация цен или каталога Digiseller | служебный |
| `POST` | `/api/locale` | Смена языка интерфейса | публичный |

## POST /api/auth/register

Создает нового пользователя.

### Request body

```json
{
  "email": "user@example.com",
  "password": "password123",
  "name": "User"
}
```

### Ограничения

- `email` должен быть корректным email;
- `password` от 6 до 200 символов;
- `name` необязательный, до 100 символов;
- rate limit: 10 запросов в минуту на IP.

### Responses

`200 OK`

```json
{
  "ok": true,
  "userId": "clx..."
}
```

`400 Bad Request`

```json
{ "ok": false, "error": "invalid" }
```

`409 Conflict`

```json
{ "ok": false, "error": "email_taken" }
```

`429 Too Many Requests`

```json
{ "ok": false, "error": "rate_limited" }
```

## POST /api/checkout/create-payment

Создает заказ по текущей корзине и возвращает ссылку для перехода к оплате через
Digiseller.

### Request body

```json
{
  "email": "buyer@example.com",
  "locale": "ru"
}
```

### Response

`200 OK`

```json
{
  "ok": true,
  "orderId": "clx...",
  "redirectUrl": "https://oplata.info/..."
}
```

Если заказ создан гостем, сервер дополнительно выставляет cookie
`order_access_<orderId>` и заголовок `x-order-access-hash`.

### Ошибки

- `400` - некорректное тело запроса или ошибка создания checkout;
- `429` - превышен rate limit: 20 запросов в минуту на IP.

## POST /api/digiseller/callback

Webhook для подтверждения оплаты от Digiseller. Метод проверяет подпись HMAC,
сохраняет событие webhook и переводит заказ в статус `FULFILLING`.

### Request body

```json
{
  "amount": "24.99",
  "currency": "USD",
  "invoice_id": "123456",
  "status": "paid",
  "signature": "hex_hmac_signature",
  "cf1": "order_id"
}
```

### Проверка подписи

Подпись строится по строке:

```text
amount:<amount>;currency:<currency>;invoice_id:<invoice_id>;status:<status>;
```

Секрет: `DIGISELLER_API_KEY`.

### Responses

- `200` `{ "ok": true }`
- `400` `{ "ok": false }`
- `401` `{ "ok": false, "error": "bad_signature" }`

## GET /api/digiseller/return

Маршрут возврата пользователя после оплаты.

### Query parameters

| Параметр | Описание |
| --- | --- |
| `cf1` | ID заказа |
| `status` | `success` или `fail` |
| `id_o` | ID счета Digiseller |

### Поведение

- `status=success` - заказ переводится в `FULFILLING`, создается fulfillment,
  пользователь перенаправляется на `/order/<id>`;
- `status=fail` - заказ переводится в `FAILED`, пользователь возвращается на
  `/checkout?error=payment_failed`;
- без `cf1` пользователь перенаправляется на `/`.

## GET /api/favorites

Возвращает избранные товары пользователя.

### Auth

Требуется активная сессия NextAuth.

### Query parameters

| Параметр | Описание |
| --- | --- |
| `ids` | Необязательный список ID товаров через запятую |

### Response без ids

```json
{
  "ok": true,
  "favorites": ["product_1", "product_2"]
}
```

### Response с ids

```json
{
  "ok": true,
  "liked": {
    "product_1": true,
    "product_2": false
  }
}
```

### Ошибка

`401`

```json
{ "ok": false, "error": "UNAUTHORIZED" }
```

## POST /api/favorites/toggle

Переключает состояние избранного для товара.

### Auth

Требуется активная сессия NextAuth.

### Request body

```json
{
  "productId": "product_id"
}
```

### Response

```json
{
  "ok": true,
  "liked": true
}
```

### Ошибки

- `400 BAD_REQUEST`;
- `401 UNAUTHORIZED`;
- `500 SERVER_ERROR`.

## POST /api/favorites/sync

Синхронизирует локально сохраненное избранное с аккаунтом пользователя.

### Auth

Требуется активная сессия NextAuth.

### Request body

```json
{
  "productIds": ["product_1", "product_2"]
}
```

Сервер принимает до 200 ID, удаляет дубли и добавляет только существующие товары.

### Response

```json
{
  "ok": true,
  "added": 2
}
```

## POST /api/jobs/fulfill

Служебный endpoint для обработки ожидающих выдач цифрового товара.

### Headers

| Header | Описание |
| --- | --- |
| `x-jobs-secret` | Секрет из `JOBS_SECRET` |
| `x-jobs-limit` | Необязательный лимит обработки, от 1 до 25 |

### Response

```json
{
  "ok": true,
  "processed": 10,
  "delivered": 9,
  "failed": 1
}
```

Фактический состав полей зависит от результата `processPendingFulfillments`.

### Ошибка

`401`

```json
{ "ok": false }
```

## POST /api/jobs/sync-digiseller

Служебный endpoint для синхронизации данных Digiseller.

### Headers

| Header | Описание |
| --- | --- |
| `x-jobs-secret` | Секрет из `JOBS_SECRET` |
| `x-sync-mode` | `prices` по умолчанию или `catalog` |

### Response: prices

```json
{
  "ok": true,
  "mode": "prices"
}
```

### Response: catalog

```json
{
  "ok": true,
  "mode": "catalog"
}
```

Фактический ответ дополняется полями результата синхронизации.

## POST /api/locale

Сохраняет выбранный язык интерфейса в cookie `locale`.

### Request body

```json
{
  "locale": "ru"
}
```

Допустимые значения определяются конфигурацией `src/i18n/config.ts`.

### Responses

- `200` `{ "ok": true }`
- `400` `{ "ok": false }`

## GET/POST /api/auth/[...nextauth]

Служебные маршруты NextAuth для входа, выхода, callback и получения сессии.
Конкретные подмаршруты формируются библиотекой NextAuth.

## Примеры curl

### Регистрация

```bash
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password123","name":"User"}'
```

### Создание платежа

```bash
curl -X POST http://localhost:3000/api/checkout/create-payment \
  -H "Content-Type: application/json" \
  -d '{"email":"buyer@example.com","locale":"ru"}'
```

### Смена языка

```bash
curl -X POST http://localhost:3000/api/locale \
  -H "Content-Type: application/json" \
  -d '{"locale":"ru"}'
```

### Запуск выдачи товаров

```bash
curl -X POST http://localhost:3000/api/jobs/fulfill \
  -H "x-jobs-secret: $JOBS_SECRET" \
  -H "x-jobs-limit: 10"
```

