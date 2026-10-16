# book-index-asset

开元古籍 `book-index` 的**图片资源仓**（公开）。book-index 只存结构化 JSON；Entity 条目引用的图片（目前是印章印影）放这里，book-index 先加载，本仓按需异步加载。

## 目录与命名

```
entity/{id[-3]}/{id[-2]}/{id[-1]}/{id}/{序号}.webp
```

- `id` 是 book-index 里该 Entity 的 12 位 id；分片取 id 末三位逐位一级目录，与 book-index 的 `Entity/` 分片一致。
  例：`hixi6zykf4ea`（文淵閣寶）→ `entity/4/e/a/hixi6zykf4ea/1.webp`
- 序号从 1 起的整数，一条 Entity 最多 8 张。
- book-index 里该 Entity 的 `images[].file` 就写上面这个相对路径（相对本仓根）。
- 图片文件只增不改名；换图用新序号，并同步改 book-index 里的 `images[]`。

## 限额

| 项 | 限制 |
|---|---|
| 格式 | webp |
| 长边 | ≤ 1600 px |
| 单张 | ≤ 300 KB |
| 每条 Entity | ≤ 8 张 |

## 版权与来源

- 只收**公有领域**（或 CC0）的印影。`images[].license` 取 `公有領域` 或 `CC0`，其他一律不入库。
- 每张图在 book-index 的 `images[]` 里必须写：`credit`（出处与署名：书名／册次／页，或图源机构与作者）、`license`、`source_url`（原始出处网址）。`note` 可写取得日期、裁切说明。
- 从书影裁出的印影，来源写到书名、册次、页；自行裁切、去底、转 webp 不改变版权状态，但要在 `note` 里说明做了什么。
- 来源不清、版权状态不明的图不入库。

## 校验

book-index 的 `.claude/qa/check_v2.py`（S06）会核对 `images[].file` 在本仓存在：设环境变量 `BOOK_INDEX_ASSET_ROOT`，或把本仓与 book-index 并排克隆（`../book-index-asset`）。没有本仓时 S06 自动跳过，CI 不硬依赖。

## 网站取图约定

本仓以 GitHub 原始路径或 CDN 镜像提供。URL = 资源根 + `images[].file`。网站先载 book-index 数据，再按 `images[].file` 异步取图；取不到图时页面照常显示条目文字。
