# Документация разработчика

Документация разработчика создана при помощи `TypeDoc` по ключевым модулям
проекта `Legal Store`.

## Охваченные модули

- `src/lib/cart.ts` - работа с корзиной;
- `src/lib/fulfillment.ts` - процесс выдачи цифрового товара;
- `src/lib/products.ts` - получение и фильтрация товаров;
- `src/lib/auth-options.ts` - параметры авторизации;
- `src/lib/digiseller/client.ts` - клиент интеграции Digiseller;
- `src/lib/digiseller/checkout.ts` - оформление платежа через Digiseller;
- `src/lib/digiseller/catalog.ts` - синхронизация каталога;
- `src/app/actions/cart.ts` - серверные действия корзины.

## Как открыть

Откройте файл:

`generated/index.html`

Также приложен архив:

`developer_documentation.zip`

## Использованный инструмент

Для генерации использовалась команда:

```bash
npx typedoc --entryPoints src/lib/cart.ts src/lib/fulfillment.ts src/lib/products.ts src/lib/auth-options.ts src/lib/digiseller/client.ts src/lib/digiseller/checkout.ts src/lib/digiseller/catalog.ts src/app/actions/cart.ts --out practice_10/developer_documentation/generated --tsconfig tsconfig.json --skipErrorChecking
```
