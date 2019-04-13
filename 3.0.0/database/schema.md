# CMS

--------------------------------------------------------

- [Users](#users)
- [Posts](#posts)
- [Taxonomy](#Taxonomy)
- [Comments](#comments)
- [Media](#media)

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

