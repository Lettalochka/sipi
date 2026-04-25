# Interface HTML CSS

## Назначение раздела

Этот раздел содержит HTML и CSS примеры пользовательского интерфейса Legal Store.
Код нужен для документации интерфейса: по нему видно структуру экрана, основные
блоки, классы и оформление элементов.

## Главный экран магазина

```html
<main class="store-page">
  <header class="store-header">
    <div class="logo">WinKey</div>
    <nav class="nav">
      <a href="/shop">Shop</a>
      <a href="/cart">Cart</a>
      <a href="/login">Log in</a>
      <a class="primary-link" href="/register">Create account</a>
    </nav>
  </header>

  <section class="hero">
    <div class="hero-content">
      <span class="badge">Official supply</span>
      <h1>Official digital keys. Instant delivery.</h1>
      <p>Legal digital goods sourced via official supply and Digiseller partners.</p>

      <form class="search-form">
        <input type="search" placeholder="Search products...">
        <button type="submit">Search</button>
      </form>
    </div>
  </section>
</main>
```

## Карточка товара

```html
<article class="product-card">
  <div class="product-image">Windows 11</div>
  <div class="product-content">
    <div class="product-meta">
      <span>Windows</span>
      <strong>$19.99</strong>
    </div>
    <h2>Windows 11 Pro License Key</h2>
    <div class="product-actions">
      <a href="/product/windows-11-pro">Details</a>
      <button type="button">Add to cart</button>
    </div>
  </div>
</article>
```

## CSS интерфейса

```css
body {
  margin: 0;
  font-family: "Segoe UI", Arial, sans-serif;
  color: #0f172a;
  background: #eef6f8;
}

.store-page {
  max-width: 1180px;
  margin: 0 auto;
  padding: 24px;
}

.store-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 14px 0 28px;
}

.logo {
  color: #0b4a6f;
  font-weight: 800;
}

.nav {
  display: flex;
  align-items: center;
  gap: 10px;
}

.nav a,
.product-actions a,
.product-actions button,
.search-form button {
  border: 1px solid #dbe7ee;
  border-radius: 999px;
  padding: 10px 16px;
  color: #0f172a;
  background: #ffffff;
  font-weight: 700;
  text-decoration: none;
}

.primary-link,
.product-actions button,
.search-form button {
  color: #ffffff;
  background: #0b4a6f;
  border-color: #0b4a6f;
}

.hero {
  border: 1px solid #dbe7ee;
  border-radius: 24px;
  padding: 40px;
  background: #ffffff;
  box-shadow: 0 18px 50px rgba(15, 23, 42, .08);
}

.badge {
  display: inline-flex;
  border-radius: 999px;
  padding: 6px 10px;
  color: #00735b;
  background: #e0f7ef;
  font-weight: 700;
}

.hero h1 {
  max-width: 620px;
  margin: 24px 0 16px;
  font-size: 52px;
  line-height: 1.05;
}

.search-form {
  display: flex;
  gap: 12px;
  margin-top: 28px;
}

.search-form input {
  flex: 1;
  min-width: 0;
  border: 1px solid #dbe7ee;
  border-radius: 999px;
  padding: 15px 18px;
  font-size: 16px;
}

.product-card {
  overflow: hidden;
  border: 1px solid #dbe7ee;
  border-radius: 20px;
  background: #ffffff;
}

.product-image {
  display: grid;
  place-items: center;
  height: 170px;
  color: #ffffff;
  background: linear-gradient(135deg, #0b4a6f, #129bd3);
  font-size: 26px;
  font-weight: 800;
}

.product-content {
  padding: 18px;
}

.product-meta,
.product-actions {
  display: flex;
  justify-content: space-between;
  gap: 10px;
}
```

## Результат

HTML показывает структуру интерфейса, а CSS показывает визуальное оформление:
цвета, отступы, карточки, кнопки, поиск и сетку действий.
