# DB定義書

マイグレーション: `xxxxxx_create_sushi_domain_tables.php`

---

## categories（カテゴリ）

| カラム名 | 型 | NULL | デフォルト | 備考 |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | NOT NULL | AUTO_INCREMENT | PK |
| name | VARCHAR(50) | NOT NULL | | カテゴリ名（UNIQUE） |
| sort_order | INT | NOT NULL | 0 | 表示順 |
| created_at | TIMESTAMP | NULL | NULL | |
| updated_at | TIMESTAMP | NULL | NULL | |

---

## products（商品）

| カラム名 | 型 | NULL | デフォルト | 備考 |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | NOT NULL | AUTO_INCREMENT | PK |
| name | VARCHAR(100) | NOT NULL | | 商品名 |
| price | INT UNSIGNED | NOT NULL | | 価格（税抜） |
| image_path | VARCHAR(255) | NOT NULL | '' | 画像パス |
| category_id | BIGINT UNSIGNED | NOT NULL | | FK → categories.id（CASCADE DELETE） |
| created_at | TIMESTAMP | NULL | NULL | |
| updated_at | TIMESTAMP | NULL | NULL | |

---

## seats（座席）

| カラム名 | 型 | NULL | デフォルト | 備考 |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | NOT NULL | AUTO_INCREMENT | PK |
| number | INT | NOT NULL | | 座席番号（UNIQUE） |
| created_at | TIMESTAMP | NULL | NULL | |
| updated_at | TIMESTAMP | NULL | NULL | |

---

## visits（来店）

| カラム名 | 型 | NULL | デフォルト | 備考 |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | NOT NULL | AUTO_INCREMENT | PK |
| seat_id | BIGINT UNSIGNED | NOT NULL | | FK → seats.id（CASCADE DELETE） |
| status | VARCHAR(16) | NOT NULL | 'seated' | ステータス（seated / paid 等） |
| total | INT UNSIGNED | NOT NULL | 0 | 合計金額（税抜） |
| total_with_tax | INT UNSIGNED | NOT NULL | 0 | 合計金額（税込） |
| created_at | TIMESTAMP | NULL | NULL | |
| updated_at | TIMESTAMP | NULL | NULL | |

---

## orders（注文）

| カラム名 | 型 | NULL | デフォルト | 備考 |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | NOT NULL | AUTO_INCREMENT | PK |
| visit_id | BIGINT UNSIGNED | NOT NULL | | FK → visits.id（CASCADE DELETE） |
| product_id | BIGINT UNSIGNED | NOT NULL | | FK → products.id |
| quantity | INT | NOT NULL | 1 | 注文数量 |
| price | INT | NOT NULL | | 注文時点の単価 |
| created_at | TIMESTAMP | NULL | NULL | |
| updated_at | TIMESTAMP | NULL | NULL | |

---

## ER図（概要）

```
categories
  └─< products (category_id)
        └─< orders (product_id)

seats
  └─< visits (seat_id)
        └─< orders (visit_id)
```
