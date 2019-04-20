# CMS

--------------------------------------------------------

- [Users](#users)
- [Posts](#posts)
- [Taxonomy](#taxonomy)
- [Comments](#comments)
- [Media](#media)
- [CRM](#crm)
- [Ecommerce](#ecommerce)
--------------------------------------------------------

When the kanso CMS is installed, the following database structure will exist

--------------------------------------------------------

### Users

`users`

| Column                | Type             |
|-----------------------|------------------|
| `id`                  | int(10) unsigned |
| `username`            | varchar(255)     |
| `visitor_id`          | varchar(255)     |
| `email`               | varchar(255)     |
| `hashed_pass`         | varchar(255)     |
| `name`                | varchar(255)     |
| `slug`                | varchar(255)     |
| `facebook`            | varchar(255)     |
| `twitter`             | varchar(255)     |
| `gplus`               | varchar(255)     |
| `instagram`           | varchar(255)     |
| `thumbnail_id`        | int(10) unsigned |
| `role`                | varchar(255)     |
| `description`         | text             |
| `status`              | varchar(255)     |
| `last_online`         | varchar(255)     |
| `email_notifications` | tinyint(1)       |
| `access_token`        | varchar(255)     |
| `register_key`        | varchar(255)     |
| `password_key`        | varchar(255)     |

`crm_visitors`

| Column        | Type             |
|---------------|------------------|
| `id`          | int(10) unsigned |
| `visitor_id`  | varchar(255)     |
| `ip_address`  | varchar(255)     |
| `name`        | varchar(255)     |
| `email`       | varchar(255)     |
| `last_active` | int(10) unsigned |

`crm_visits`

| Column       | Type             |
|--------------|------------------|
| `id`         | int(10) unsigned |
| `visitor_id` | varchar(255)     |
| `ip_address` | varchar(255)     |
| `page`       | varchar(255)     |
| `date`       | int(10) unsigned |
| `medium`     | varchar(255)     |
| `channel`    | varchar(255)     |
| `campaign`   | varchar(255)     |
| `keyword`    | varchar(255)     |
| `creative`   | varchar(255)     |

--------------------------------------------------------

### Posts

`posts`

| Column             | Type             |
|--------------------|------------------|
| `id`               | int(10) unsigned |
| `created`          | int(10) unsigned |
| `modified`         | int(10) unsigned |
| `status`           | varchar(255)     |
| `type`             | varchar(255)     |
| `slug`             | varchar(255)     |
| `title`            | varchar(255)     |
| `excerpt`          | text             |
| `author_id`        | int(10) unsigned |
| `thumbnail_id`     | int(10) unsigned |
| `comments_enabled` | tinyint(1)       |

`content_to_posts`

| Column    | Type             |
|-----------|------------------|
| `id`      | int(10) unsigned |
| `content` | text             |
| `post_id` | int(10) unsigned |

`post_meta`

| Column    | Type             |
|-----------|------------------|
| `id`      | int(10) unsigned |
| `content` | text             |
| `post_id` | int(10) unsigned |

--------------------------------------------------------

### Taxonomy

`categories`

| Column        | Type             |
|---------------|------------------|
| `id`          | int(10) unsigned |
| `name`        | varchar(255)     |
| `slug`        | varchar(255)     |
| `description` | text             |
| `parent_id`   | int(10) unsigned |

`categories_to_posts`

| Column        | Type             |
|---------------|------------------|
| `id`          | int(10) unsigned |
| `category_id` | int(10) unsigned |
| `post_id`     | int(10) unsigned |

`tags`

| Column        | Type             |
|---------------|------------------|
| `id`          | int(10) unsigned |
| `name`        | varchar(255)     |
| `slug`        | varchar(255)     |
| `description` | text             |

`tags_to_posts`

| Column    | Type             |
|-----------|------------------|
| `id`      | int(10) unsigned |
| `tag_id`  | int(10) unsigned |
| `post_id` | int(10) unsigned |


--------------------------------------------------------

### Comments

`comments`

| Column         | Type             |
|----------------|------------------|
| `id`           | int(10) unsigned |
| `parent`       | int(10) unsigned |
| `post_id`      | int(10) unsigned |
| `date`         | int(10) unsigned |
| `type`         | varchar(255)     |
| `status`       | varchar(255)     |
| `name`         | varchar(255)     |
| `email`        | varchar(255)     |
| `content`      | text             |
| `html_content` | text             |
| `ip_address`   | varchar(255)     |
| `email_reply`  | tinyint(1)       |
| `email_thread` | tinyint(1)       |
| `rating`       | int(11)          |

--------------------------------------------------------

### Media

`media_uploads`

| Column        | Type             |
|---------------|------------------|
| `id`          | int(10) unsigned |
| `url`         | varchar(255)     |
| `path`        | varchar(255)     |
| `title`       | varchar(255)     |
| `alt`         | varchar(255)     |
| `size`        | int(10) unsigned |
| `dimensions`  | varchar(255)     |
| `date`        | int(10) unsigned |
| `uploader_id` | int(10) unsigned |

--------------------------------------------------------

### CRM

`crm_visitors`

| Column          | Type             |
|-----------------|------------------|
| `id`            | int(10) unsigned |
| `visitor_id`    | varchar(255)     |
| `ip_address`    | varchar(255)     |
| `name`          | varchar(255)     |
| `email`         | varchar(255)     |
| `made_purchase` | tinyint(1)       |
| `last_active`   | int(10) unsigned |

`crm_visits`

| Column         | Type              |
|----------------|-------------------|
|`id`             | int(10) unsigned |
|`visitor_id`     | varchar(255)     |
|`ip_address`     | varchar(255)     |
|`page`           | varchar(255)     |
|`date`           | int(10) unsigned |
|`end`            | int(10) unsigned |
|`medium`         | varchar(255)     |
|`channel`        | varchar(255)     |
|`campaign`       | varchar(255)     |
|`keyword`        | varchar(255)     |
|`creative`       | varchar(255)     |
|`browser`        | varchar(255)     |

`crm_visit_actions`

| Column              | Type              |
|---------------------|-------------------|
|`id`                 | int(10) unsigned  |
|`visit_id`           | int(10) unsigned  |
|`visitor_id`         | varchar(255)      |
|`action_name`        | varchar(255)      |
|`action_description` | varchar(255)      |
|`page`               | varchar(255)      |
|`date`               | varchar(255)      |

--------------------------------------------------------

### Ecommerce

`shopping_cart`

| Column              | Type              |
|---------------------|-------------------|
|`id`                 | int(10) unsigned  |
|`user_id`            | int(10) unsigned  |
|`product_id`         | int(10) unsigned  |
|`offer_id`           | varchar(255)      |
|`quantity`           | int(10) unsigned  |


`shipping_address`

| Column                | Type              |
|-----------------------|-------------------|
| `id`                  | int(10) unsigned  |
| `user_id`  	        | int(10) unsigned  |
| `email`               | varchar(255)      |
| `first_name`          | varchar(255)      |
| `last_name`           | varchar(255)      |
| `street_address_1`    | varchar(255)      |
| `street_address_2`    | varchar(255)      |
| `suburb`              | varchar(255)      |
| `zip_code`            | varchar(255)      |
| `state`               | varchar(255)      |
| `country`             | varchar(255)      |
| `telephone`           | varchar(255)      |

`transactions`

| Column              | Type                 |
|---------------------|----------------------|
| `id`                | int(10) unsigned     |
| `user_id`           | integer(11) unsigned |
| `bt_transaction_id` | varchar(255)         |
| `shipping_id`       | integer(11) unsigned |
| `date`              | integer(11) unsigned |
| `status`            | varchar(255)         |
| `shipped`           | tinyint(1) default 0 |
| `shipped_date`      | integer(11) unsigned |
| `tracking_code`     | varchar(255)         |
| `eta`               | integer(11) unsigned |
| `card_type`         | varchar(255)         |
| `card_last_four`    | integer(11) unsigned |
| `card_expiry`       | varchar(255)         |
| `items`             | TEXT                 |
| `sub_total`         | varchar(255)         |
| `shipping_costs`    | varchar(255)         |
| `coupon`            | varchar(255)         |
| `total`             | varchar(255)         |


`loyalty_points`

| Column         | Type                 |
|----------------|----------------------|
| `id`           | int(10) unsigned     |
| `user_id`      | integer(11) unsigned |
| `description`  | varchar(255)         |
| `date`         | integer(11) unsigned |
| `points_add`   | integer(11) unsigned |
| `points_minus` | integer(11) unsigned |


`loyalty_coupons`

| Column        | Type                 |
|---------------|----------------------|
| `id`          | int(10) unsigned     |
| `user_id`     | integer(11) unsigned |
| `name`        | varchar(255)         |
| `description` | varchar(255)         |
| `discount`    | integer(11) unsigned |
| `code`        | varchar(255)         |
| `date`        | integer(11) unsigned |
| `used`        | tinyint(1) default 0 |


`product_reviews`

| Column        | Type                 |
|---------------|----------------------|
| `id`          | int(10) unsigned     |
| `comment_id`  | integer(11) unsigned |
| `product_id`  | integer(11) unsigned |
| `rating`      | integer(11) unsigned |
| `recommended` | tinyint(1) default 0 |


`product_review_votes`

| Column       | Type                 |
|--------------|----------------------|
| `id`         | int(10) unsigned     |
| `comment_id` | integer(11) unsigned |
| `up_vote`    | tinyint(1) default 0 |
| `ip_address` | varchar(255)         |


`used_public_coupons`

| Column        | Type                 |
|---------------|----------------------|
| `id`          | int(10) unsigned     |
| `user_id`     | integer(11) unsigned |
| `email`       | varchar(255)         |
| `coupon_name` | varchar(255)         |

