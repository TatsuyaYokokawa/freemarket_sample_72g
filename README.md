# README

## usersテーブル

|Column         |Type  |Options                  |
|---------------|------|-------------------------|
|nickname       |string|null: false, unique: true|
|email          |string|null: false, unique: true|
|password       |string|null: false              |
|last_name      |string|null: false              |
|first_name     |string|null: false              |
|last_name_kana |string|null: false              |
|first_name_kana|string|null: false              |
|birth_year     |string|null: false              |
|birth_month    |string|null: false              |
|birth_day      |string|null: false              |
|postcode       |string|null: false              |
|prefecture     |string|null: false              |
|city           |string|null: false              |
|block          |string|null: false              |
|phone_number   |string|
|image          |string|

### Association
- has_many :buyed_goods, foreign_key: "buyer_id", class_name: "Good", dependent: :destroy
- has_many :selling_goods, -> { where("buyer_id is NULL") }, foreign_key: "seller_id", class_name:  "Good", dependent: :destroy
- has_many :sold_goods, -> { where("buyer_id is not NULL") }, foreign_key: "seller_id", class_name: "Good", dependent: :destroy
- has_many :comments, through: :users_comments, dependent: :destroy
- has_many :users_comments, dependent: :destroy
- has_many :orders
- has_many :user_address
- belongs_to :card, dependent: :destroy

## user_addressテーブル

|Column         |Type   |Options                       |
|---------------|-------|------------------------------|
|user_id        |integer|null: false, foreign_key: true|
|last_name      |string |null: false                   |
|first_name     |string |null: false                   |
|last_name_kana |string |null: false                   |
|first_name_kana|string |null: false                   |
|postcode       |string |null: false                   |
|prefecture     |string |null: false                   |
|city           |string |null: false                   |
|block          |string |null: false                   |

### Association
- has_many   :orders
- has_one :user

## cardsテーブル

|Column     |Type   |Options    |
|-----------|-------|-----------|
|user_id    |integer|null: false|
|customer_id|string |null: false|
|card_id    |string |null: false|

### Association
- has_one :user

## commentsテーブル

|Column|Type    |Options          |
|-------|-------|-----------------|
|text   |string |
|good_id|integer|foreign_key: true|
|user_id|integer|foreign_key: true|

### Association
- has_many :users, through: :users_comments
- has_many :users_comments
- belongs_to :good

## users_commentsテーブル

|Column |Type   |Options                       |
|-------|-------|------------------------------|
|user_id|integer|null: false, foreign_key: true|
|good_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :comment
- belongs_to :user

## goodsテーブル

|Column               |Type      |Options                                       |
|---------------------|----------|----------------------------------------------|
|name                 |string    |null: false                                   |
|state                |string    |null: false                                   |
|region               |string    |null: false                                   |
|postage              |string    |null: false                                   |
|shipping_date        |string    |null: false                                   |
|expanation           |string    |null: false                                   |
|trading_conditions_id|integer   |null: false, default: 1                       |
|price                |integer   |null: false                                   |
|delivery_method_id   |integer   |null: false, foreign_key: true                |
|size_id              |integer   |null: false, foreign_key: true                |
|category_id          |bigint    |null: false, foreign_key: true                |
|seller               |references|null: false, foreign_key: { to_table: :users }|
|buyer                |references|foreign_key: { to_table: :users }             |
      
### Association
- belongs_to :seller, class_name: "User"
- belongs_to :buyer, class_name: "User", optional: true
- has_one :category
- has_many :orders
- has_many :comments, dependent: :destroy
- has_many :good_images, dependent: :destroy
- accepts_nested_attributes_for :good_images, allow_destroy: true

## ordersテーブル

|Column         |Type   |Options                       |
|---------------|-------|------------------------------|
|user_id        |integer|null: false, foreign_key: true|
|good_id        |integer|null: false, foreign_key: true|
|user_address_id|integer|null: false, foreign_key: true|

### Association
- belongs_to :user
- belongs_to :user_address
- belongs_to :good

## good_imagesテーブル

|Column|Type  |Options    |
|------|------|-----------|
|image |string|null: false|

### Association
- belongs_to :good

## categoriesテーブル


|Column  |Type  |Options    |
|--------|------|-------    |
|name    |string|null: false|
|ancestry|string|null: false|

### Association
- has_one :good
- has_ancestry

