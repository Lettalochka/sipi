# Интерфейс пользователя

В пользовательскую Wiki включен HTML-код интерфейса, чтобы документация показывала
структуру экранов без скриншотов. Полную HTML-страницу можно открыть из файла
`user_interface.html`.

## Верхняя панель

```html
<header class="header">
  <nav class="nav">
    <div class="logo">WinKey</div>
    <button class="button">Shop</button>
  </nav>
  <div class="actions">
    <span class="pill">Official supply</span>
    <button class="icon-button" aria-label="Language">EN</button>
    <button class="icon-button" aria-label="Cart">Cart</button>
    <button class="button">Log in</button>
    <button class="button primary">Create account</button>
  </div>
</header>
```

## Поиск и первый экран

```html
<section class="hero">
  <div class="panel">
    <span class="pill">Official supply - Processed via Digiseller</span>
    <h1>Official digital keys. Instant delivery.</h1>
    <p class="lead">Legal digital goods sourced via official supply and Digiseller partners.</p>
    <form class="search">
      <input type="search" placeholder="Search products...">
      <button class="button primary" type="submit">Search</button>
    </form>
  </div>
</section>
```

## Карточка товара

```html
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
```

## Минимальные стили интерфейса

```html
<style>
  .page {
    max-width: 1180px;
    margin: 0 auto;
    padding: 24px;
    font-family: "Segoe UI", Arial, sans-serif;
  }

  .header,
  .actions,
  .nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
  }

  .panel,
  .product-card {
    background: #ffffff;
    border: 1px solid #dbe7ee;
    border-radius: 20px;
  }

  .button {
    border: 1px solid #dbe7ee;
    border-radius: 999px;
    padding: 10px 16px;
    font-weight: 700;
  }

  .button.primary {
    color: #ffffff;
    background: #0b4a6f;
    border-color: #0b4a6f;
  }
</style>
```
