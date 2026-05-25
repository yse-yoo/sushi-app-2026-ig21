# API ルーティング一覧

## `/api/*` — フロントエンド向け REST API

| メソッド | エンドポイント | コントローラー | アクション |
|--------|--------------|--------------|----------|
| GET | `/api/category/fetch` | `CategoryController` | `index` |
| GET | `/api/product/fetch` | `ProductController` | `index` |
| GET | `/api/seat/fetch` | `SeatController` | `index` |
| GET | `/api/seat/available` | `SeatController` | `available` |
| GET | `/api/seat/find` | `SeatController` | `show` |
| GET | `/api/visit/find` | `VisitController` | `show` |
| POST | `/api/visit/join` | `VisitController` | `join` |
| GET | `/api/order/fetch` | `OrderController` | `index` |
| POST | `/api/order/add` | `OrderController` | `store` |
| POST | `/api/order/billed` | `OrderController` | `bill` |

---

## `/admin/*` — 管理画面 Web ルート

| メソッド | エンドポイント | コントローラー | アクション | ルート名 |
|--------|--------------|--------------|----------|---------|
| GET | `/` | — | `welcome` view | — |
| GET | `/admin` | `DashboardController` | `index` | `admin.dashboard` |
| GET | `/admin/category` | `CategoryController` | `index` | `admin.category.index` |
| GET | `/admin/category/create` | `CategoryController` | `create` | `admin.category.create` |
| POST | `/admin/category` | `CategoryController` | `store` | `admin.category.store` |
| GET | `/admin/category/edit/{category}` | `CategoryController` | `edit` | `admin.category.edit` |
| POST | `/admin/category/update/{category}` | `CategoryController` | `update` | `admin.category.update` |
| POST | `/admin/category/delete/{category}` | `CategoryController` | `destroy` | `admin.category.destroy` |
| GET | `/admin/product` | `ProductController` | `index` | `admin.product.index` |
| GET | `/admin/product/create` | `ProductController` | `create` | `admin.product.create` |
| POST | `/admin/product/store` | `ProductController` | `store` | `admin.product.store` |
| GET | `/admin/product/edit/{product}` | `ProductController` | `edit` | `admin.product.edit` |
| POST | `/admin/product/update/{product}` | `ProductController` | `update` | `admin.product.update` |
| POST | `/admin/product/delete/{product}` | `ProductController` | `destroy` | `admin.product.destroy` |
| GET | `/admin/seat` | `SeatController` | `index` | `admin.seat.index` |
| GET | `/admin/seat/edit/{seat}` | `SeatController` | `edit` | `admin.seat.edit` |
| POST | `/admin/seat/update/{seat}` | `SeatController` | `update` | `admin.seat.update` |
| GET | `/admin/visit` | `VisitController` | `index` | `admin.visit.index` |
| GET | `/admin/visit/show/{visit}` | `VisitController` | `show` | `admin.visit.show` |
| GET/POST | `/admin/database` | `DatabaseController` | `index` | `admin.database` |

---

## フロントエンド fetch 対応表

| メソッド | エンドポイント | サービスファイル | 関数名 | 実装状態 |
|--------|--------------|--------------|--------|---------|
| GET | `/api/category/fetch` | `categoryService.js` | `loadCategories()` | fetch 呼び出しがコメントアウト済み、`response` 未定義で実行時エラー |
| GET | `/api/product/fetch` | `productService.js` | `loadProducts(categoryId)` | 実装済み |
| GET | `/api/seat/fetch` | `seatService.js` | `loadSeats(selectedSeatId, selectedSeatNumber)` | 実装済み |
| GET | `/api/seat/available` | — | — | フロントエンド未実装 |
| GET | `/api/seat/find` | — | — | フロントエンド未実装 |
| GET | `/api/visit/find` | `visitService.js` | `findVisitById(visitId)` | 実装済み |
| POST | `/api/visit/join` | `visitService.js` | `joinVisit(seatId)` | 実装済み |
| GET | `/api/order/fetch` | `orderService.js` | `loadOrders(visitId)` | 実装済み |
| POST | `/api/order/add` | `orderService.js` | `submitOrder(visitId, product, quantity)` | **未完成**：URL・method・body が未設定 |
| POST | `/api/order/billed` | `orderService.js` | `checkoutOrder(visitId)` | 実装済み |

### 要対応箇所

1. **`categoryService.js` — `loadCategories()`**
   - `fetch(url, options)` と `response.json()` がコメントアウトされており、`response` 変数が未定義のまま参照されるため実行時エラーになる

2. **`orderService.js` — `submitOrder()`**
   - URL: `${apiBaseUrl}` → `${apiBaseUrl}/api/order/add` に修正が必要
   - `method: ''` → `'POST'` に修正が必要
   - `headers: { 'Content-Type': '' }` → `'application/json'` に修正が必要
   - `body: JSON.stringify()` → `JSON.stringify({ product_id: productId, quantity, visit_id: Number(visitId) })` に修正が必要

3. **`/api/seat/available`・`/api/seat/find`**
   - バックエンドにルートは存在するが、フロントエンドの対応サービス関数がない（現時点では未使用）
