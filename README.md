# huoju-data
知乎、微博热榜数据收集

最新数据请前往 https://github.com/zzjcool/huoju-data/releases 下载

实时数据查看：https://huoju.info/

---

## 数据库文档

### 表: HotType

**用途**: 存储热榜类型的基本信息。

| 字段名       | 数据类型    | 索引         | 注释     |
|--------------|-------------|--------------|----------|
| id           | bigint      | primary key  | 主键     |
| type_name    | varchar(16) | unique index | 站点名称 |
| refresh_time | int         |              | 刷新时间 |

### 表: Hot

**用途**: 存储具体的热榜条目。

| 字段名       | 数据类型    | 索引                   | 注释     |
|--------------|-------------|------------------------|----------|
| id           | bigint      | primary key            | 主键     |
| type_id      | int         | unique index (type_key)| 热榜类型 |
| unique_key   | varchar(64) | unique index (type_key)| 唯一标识 |
| title        | varchar(255)| index (idx_title)      | 标题     |
| content      | text        |                        | 内容     |
| link         | varchar(255)|                        | 链接     |
| image_link   | varchar(255)|                        | 图片链接 |

### 表: Rank

**用途**: 存储热榜条目的排名和热度指标。

| 字段名       | 数据类型    | 索引                           | 注释     |
|--------------|-------------|--------------------------------|----------|
| id           | bigint      | primary key                    | 主键     |
| type_id      | int         | index (idx_typename_timestamp) | 站点名称 |
| unique_key   | varchar(64) | index                          | 唯一标识 |
| rank         | tinyint     |                                | 排名     |
| metric       | int         |                                | 热度指标 |
| timestamp    | int         | index (idx_typename_timestamp) | 时间戳   |

---

### 详细说明

- **HotType 表**
  - `id`: 主键，自动生成。
  - `type_name`: 站点名称，长度为16个字符，唯一索引。
  - `refresh_time`: 刷新时间，整数类型。

- **Hot 表**
  - `id`: 主键，自动生成。
  - `type_id`: 热榜类型，整数类型，和 `unique_key` 组成唯一索引。
  - `unique_key`: 唯一标识，长度为64个字符，和 `type_id` 组成唯一索引。
  - `title`: 标题，长度为255个字符，索引 `idx_title`。
  - `content`: 内容，文本类型。
  - `link`: 链接，长度为255个字符。
  - `image_link`: 图片链接，长度为255个字符。

- **Rank 表**
  - `id`: 主键，自动生成。
  - `type_id`: 站点名称，整数类型，索引 `idx_typename_timestamp`。
  - `unique_key`: 唯一标识，长度为64个字符，索引。
  - `rank`: 排名，tinyint 类型。
  - `metric`: 热度指标，整数类型。
  - `timestamp`: 时间戳，整数类型，索引 `idx_typename_timestamp`。

---
