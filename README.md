# 番茄小说 Legado 书源（含段评）

一个兼容 Legado 书源格式的番茄小说书源：搜索、详情、目录、正文，以及按段落查看评论、翻页与回复。

> 仅用于个人学习与阅读便利。内容版权归原作者及平台所有，请遵守番茄小说用户协议与当地法律。

## 重要说明

**该源需要 [Ceres](https://github.com/jol888/ceres) 阅读器。** 原版 Legado 无法直接使用。

书源里的详情、目录、正文与段评地址使用 `fanqie://` 合成地址，由 Ceres 内置的原生番茄客户端解析：设备注册、请求签名、正文解密和评论接口都在客户端完成。搜索则直接使用公开的 `novel.snssdk.com` 接口，不依赖客户端。

## 用法

1. 安装 [Ceres](https://github.com/jol888/ceres)（`app/build/outputs/apk/debug/app-debug.apk`）。
2. 打开「书源」→「导入」，选择本仓库的 `fanqie_novel_source.json`，或把 JSON 内容粘贴到剪贴板后导入。
3. 搜索任意书名（示例：《十日终焉》），打开书籍即可阅读。
4. 阅读时打开「显示段评」，段尾会显示评论数，点开可看评论与回复。

首次请求正文时会自动注册设备身份并获取正文密钥，之后会复用缓存；密钥失效时会自动重新注册。

## 书源内容

| 规则 | 来源 |
| --- | --- |
| 搜索 | `https://novel.snssdk.com/api/novel/channel/homepage/search/search/v1/` |
| 详情 | `fanqie://book/<book_id>`（原生客户端） |
| 目录 | `fanqie://toc/<book_id>`（公开 web 目录接口） |
| 正文 | `fanqie://chapter/<book_id>/<item_id>`（原生客户端） |
| 段评统计 | `fanqie://reviews/summary` |
| 段评列表 | `fanqie://reviews/<para_index>/<page>` |
| 评论回复 | `fanqie://replies/<comment_id>/<page>` |

段评统计会跳过整章汇总（索引 0），只把有评论的段落交给阅读器；评论列表与回复由客户端归一化成 Ceres 的段评协议。

## 实测

在 Ceres 的联调用例中，使用本书源完成：

- 搜索《十日终焉》，命中 10 本
- 详情：十日终焉 / 杀虫队队员 / 320.1 万字
- 目录：1496 章
- 正文：第 1 章 68 段 / 2418 字
- 段评：66 段、共 96333 条；单段评论可翻页，回复可解析

复跑：

```bash
cd ceres
CERES_LIVE=1 CERES_FANQIE_SOURCE=/path/to/fanqie_novel_source.json \
  ./gradlew :app:testDebugUnitTest \
  --tests 'app.ceres.reader.source.fanqie.FanqieLiveTest'
```

## 许可证

[GNU Affero General Public License v3.0](LICENSE)（AGPL-3.0）。
