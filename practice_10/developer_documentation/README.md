# Документация разработчика

Документация разработчика для проекта `WinKey` оформлена как API-reference,
поскольку основная серверная логика доступна через Next.js API routes.

## Основные файлы

- `api_reference.md` - подробное описание API: методы, endpoint, параметры,
  заголовки, тела запросов, ответы и ошибки.
- `api_reference.html` - HTML-страница API-reference для просмотра в браузере.
- `openapi.yaml` - спецификация OpenAPI 3.0 для маршрутов проекта.
- `developer_documentation.zip` - архив с дополнительной HTML-документацией,
  сгенерированной по TypeScript-модулям.

## Описанные группы API

- Auth API: регистрация и NextAuth-маршруты.
- Checkout API: создание платежа.
- Digiseller API: callback и return URL.
- Favorites API: получение, переключение и синхронизация избранного.
- Jobs API: обработка выдачи товара и синхронизация Digiseller.
- Locale API: смена языка интерфейса.

## Как использовать

Для чтения в браузере откройте `api_reference.html`.

Для импорта в инструменты разработки API используйте `openapi.yaml`, например в
Swagger Editor, Postman или Insomnia.
