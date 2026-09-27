# pstoh.github.io

个人技术博客（中英双语）：爬虫、代理IP 与数据采集实战笔记。
线上地址：<https://pstoh.github.io/> ｜ 英文版：<https://pstoh.github.io/en/> ｜ RSS：<https://pstoh.github.io/feed.xml>

纯静态站点，零依赖——没有构建步骤，没有框架，没有 CDN，改完直接推。

## 目录结构

```
.
├── index.html                  中文首页（文章列表）
├── about.html                  关于
├── 404.html                    404 页
├── posts/                      中文文章
│   ├── how-http-proxy-works.html          HTTP 代理原理
│   ├── proxy-pool-design.html             代理池设计
│   ├── proxy-error-troubleshooting.html   错误码排查
│   ├── scrapy-proxy-middleware.html       Scrapy 中间件
│   └── crawler-rate-limit.html            限速与合规
├── en/                         英文版（与中文一一对应，同文件名）
│   ├── index.html
│   ├── about.html
│   └── posts/ …                同上的 5 篇英文文章
├── assets/
│   ├── style.css               全站样式（支持深色模式）
│   ├── favicon.svg             站点图标
│   └── og-cover.png            社交分享封面 1200x630
├── feed.xml                    RSS 2.0 订阅源
├── <indexnow-key>.txt          IndexNow 密钥文件（向 Bing / Yandex 提交 URL 用）
├── robots.txt
├── sitemap.xml                 含 xhtml:link 多语言标注
└── .nojekyll                   跳过 Jekyll 处理
```

## 部署

仓库名必须是 `pstoh.github.io`，推到 `main` 分支后 GitHub Pages 会自动发布，几十秒后访问 <https://pstoh.github.io/> 即可。

```bash
git init -b main
git add -A
git commit -m "feat: 初始化技术博客"
git remote add origin https://github.com/pstoh/pstoh.github.io.git
git push -u origin main
```

在仓库 Settings → Pages 里确认 Source 是 `Deploy from a branch`、分支为 `main` / `(root)`。

## 已做的 SEO 处理

- 每页独立的 `title` / `description` / `keywords` / `canonical`
- **中英双语 hreflang 互指**：`zh-CN`、`en`、`x-default`（x-default 指向中文原文），sitemap 里同步用 `xhtml:link` 声明
- Open Graph 与 Twitter Card，配 1200×630 分享图
- 结构化数据：`BlogPosting`、`BreadcrumbList`、`WebSite`、`Person`、`ItemList`
- RSS 2.0 订阅源，每页 `<head>` 里都有 `rel="alternate"` 声明，页脚有入口
- 语义化标签（`header` / `nav` / `main` / `article` / `time`），文章带 `datetime`
- 文章之间互链，首页与关于页互链，中英文页面互相跳转
- `sitemap.xml` + `robots.txt`，并已在 robots 里声明 sitemap
- 移动端自适应 + `prefers-color-scheme` 深色模式
- 单文件 CSS，无外部字体和 JS，首屏体积小

## 新增一篇文章

1. 复制 `posts/proxy-pool-design.html`，改掉 `title`、`description`、`canonical`、`og:*` 与正文；
2. 同步改 JSON-LD 里的 `headline` / `datePublished` / `dateModified` / `articleSection`；
3. 在 `index.html` 的 `.cards` 里加一张卡片，并更新 JSON-LD 的 `ItemList`；
4. 在 `sitemap.xml` 里加对应 `<url>` 块（中英各一条，带三条 `xhtml:link`），`lastmod` 用当天日期；
5. 在其它文章的「接着看」里加上新链接；
6. 如果有英文版，把英文文件放到 `en/posts/` 下（同名），并检查两边的 `hreflang` 是否互指；
7. 在 `feed.xml` 的 `<channel>` 里加一条 `<item>`，`pubDate` 用 RFC 822 格式。

## 搜索引擎提交

- 已向 **IndexNow**（Bing / Yandex / Seznam / Naver 共用）提交全站 14 条 URL
- 密钥文件：`f796f07a0d987723dfe8c7840240cb39.txt`（仓库根目录，内容即密钥本身），重新提交时按下面格式 POST：

```
POST https://api.indexnow.org/indexnow
Content-Type: application/json; charset=utf-8

{"host":"pstoh.github.io","key":"f796f07a0d987723dfe8c7840240cb39","keyLocation":"https://pstoh.github.io/f796f07a0d987723dfe8c7840240cb39.txt",
 "urlList":["https://pstoh.github.io/", "..."]}
```

- 百度 / Google 需要在各自站长平台验证站点归属（HTML 验证文件或 meta 标签），验证码拿到后放进本仓库根目录 / 首页 `<head>` 即可

## 许可

文章内容 CC BY 4.0，站内代码示例可自由用于商业项目。