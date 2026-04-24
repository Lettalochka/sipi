# Каталог товаров

Каталог позволяет просматривать цифровые товары, выполнять поиск и выбирать
подходящий товар.

## HTML-код каталога

```html
<section class="product-grid" aria-label="Product list">
  <article class="product-card">
    <div class="product-cover">Windows 11</div>
    <div class="product-body">
      <div class="meta">
        <span>Windows</span>
        <strong>$19.99</strong>
      </div>
      <p class="product-title">Windows 11 Pro License Key</p>
      <div class="card-actions">
        <button class="button">Details</button>
        <button class="button primary">Add to cart</button>
      </div>
    </div>
  </article>
</section>
```

## Основные действия

1. Открыть страницу каталога.
2. Использовать поиск по названию.
3. Выбрать категорию.
4. Отсортировать товары по цене, популярности или новизне.
5. Открыть карточку товара для подробного просмотра.

## Карточка товара

В карточке товара отображаются:

- название;
- описание;
- цена;
- изображения;
- платформа;
- издание;
- доступность товара;
- кнопка добавления в корзину.

Кнопка `Details` открывает подробную страницу товара, а кнопка `Add to cart`
добавляет выбранную позицию в корзину.
