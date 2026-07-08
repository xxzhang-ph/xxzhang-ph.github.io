# 张骁骁学术主页 - 技术文档

## 项目概述

这是一个用于 GitHub Pages 的个人学术主页，服务于华中科技大学国家脉冲强磁场科学中心和物理学院的张骁骁教授。站点为纯静态单页应用，支持中英双语切换。

- **仓库地址**: `https://github.com/xxzhang-ph/xxzhang-ph.github.io`
- **线上地址**: `https://xxzhang-ph.github.io`
- **部署方式**: GitHub Pages（直接从 main 分支的 `index.html` 提供服务）

---

## 文件结构

```
.
├── index.html          # 主页面（所有内容）
├── style.css           # 所有样式
├── assets/
│   └── photo.jpg       # 个人照片（248×298 像素，约 5:6 比例）
├── sitemap.xml         # 站点地图（SEO）
├── robots.txt          # 爬虫指令（SEO）
├── TECHNICAL_DOC.md    # 本技术文档
└── .git/
```

无构建步骤，无依赖管理，纯静态文件。

---

## 核心机制

### 1. 双语系统

页面通过 `data-zh` 和 `data-en` 属性实现中英双语切换。

**HTML 格式：**
```html
<元素 data-zh="中文内容" data-en="English content">中文内容</元素>
```

**切换逻辑（内联 JS）：**
```javascript
let currentLang = 'zh';

function toggleLang() {
  currentLang = currentLang === 'zh' ? 'en' : 'zh';
  document.querySelectorAll('[data-zh]').forEach(el => {
    el.textContent = el.getAttribute('data-' + currentLang);
  });
  document.documentElement.lang = currentLang === 'zh' ? 'zh-CN' : 'en';
}
```

**重要规则：任何内容修改必须同时更新 `data-zh` 和 `data-en` 两个属性，以及元素内的默认文本（默认显示中文）。**

切换按钮固定在右上角：
```html
<button class="lang-toggle" onclick="toggleLang()">中 / EN</button>
```

### 2. CSS 缓存破坏

由于 GitHub Pages 会缓存静态资源，CSS 修改后需要更新版本号：
```html
<link rel="stylesheet" href="style.css?v=y1a2b">
```
每次修改 CSS 后，更改 `?v=` 后面的值（任意字符串均可）。

### 3. 访客统计（不蒜子 Busuanzi）

使用不蒜子（busuanzi.ibruce.info）进行访问统计，这是国内可用的免费服务。

**嵌入代码（位于 `</footer>` 之后）：**
```html
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

**显示元素（位于 `<footer>` 内）：**
```html
<p class="visitor-stats">
  <span data-zh="本站总访问量" data-en="Total Page Views">本站总访问量</span>
  <span id="busuanzi_value_site_pv"></span>
  <span data-zh="次 · 总访客数" data-en=" · Unique Visitors">次 · 总访客数</span>
  <span id="busuanzi_value_site_uv"></span>
  <span data-zh="人" data-en="">人</span>
</p>
```

- `#busuanzi_value_site_pv` — 页面浏览量
- `#busuanzi_value_site_uv` — 独立访客数

---

## 页面结构（index.html）

按顺序排列的区块：

| 区块 ID | 内容 | 说明 |
|---------|------|------|
| `#hero` | 姓名、职位、单位、社交链接、照片 | 顶部区域，flex 布局，照片在右侧 |
| `#about` | 个人简介 | 一段文字，中英双语 |
| `#research` | 研究方向 | 3 个方向 + 方法论描述 |
| `#publications` | 论文 | 30余篇，18篇见于一区TOP期刊 + ORCID 链接 |
| `#education` | 教育与工作经历 | 时间线格式（`.timeline`） |
| `#teaching` | 教学 | 课程标签 |
| `#recruitment` | 招生招聘 | 特殊背景色区块 |
| `#contact` | 联系方式 | 邮箱、地址、单位 |

---

## 关键 CSS 样式

### 整体风格
- 字体：Georgia, Times New Roman, serif
- 背景色：`#faf8f5`（暖米色）
- 主色调：`#6b5b3e`、`#8b7355`（棕色系）
- 内容最大宽度：800px，居中

### 照片区域（已压缩调整）

```css
#hero {
  text-align: left;
  padding: 129px 0 89px;  /* 原版为 120px 0 80px，已压缩 1/12.5 */
}

.profile-photo {
  width: 180px;
  height: 202px;           /* 原版为 220px，已压缩 1/12.5 */
  border-radius: 50%;      /* 椭圆镜框 */
  object-fit: cover;       /* 填满容器，裁剪超出部分 */
  object-position: center; /* 关键：照片居中，压缩时上下均匀裁剪 */
  border: 3px solid #d4c5a9;
  flex-shrink: 0;
  margin-top: 30px;        /* 下移，使照片顶端不高于"张骁骁"文字 */
}
```

**压缩原理**：原版高度 220px，压缩 1/12.5 后为 202px。为保持镜框中心位置不变，上下各增加 9px padding（从 120/80 变为 129/89）。`object-position: center` 确保压缩时照片从上下两端均匀裁剪，而非固定顶部。

### 教学课程标签

```css
#teaching li {
  padding: 12px 28px;
  background: #f5f0e8;
  border: 1px solid #d4c5a9;
}
```

### 招生招聘区块（特殊背景）

```css
#recruitment {
  background: #f5f0e8;
  margin: 0 -40px;         /* 负 margin 突破 main 容器 */
  padding: 80px 64px !important;
}
```

---

## 已完成的修改记录

### 1. 基础信息添加
- 在 hero 区域的单位行前添加"华中科技大学"（中英文均已添加）
- 在简介（About）段落中，在"国家脉冲强磁场科学中心"前添加"华中科技大学"

### 2. 照片镜框压缩
- 原版高度 220px → 202px（压缩 1/12.5）
- hero 区域上下 padding 各增加 9px（120/80 → 129/89）以保持中心不变
- `object-position` 从 `top` 改为 `center`，确保压缩时上下均匀裁剪
- 添加 `margin-top: 30px` 使照片顶端不高于"张骁骁"文字

### 3. 教育经历修正
- RIKEN 职位英文从 "Special Postdoctoral Researcher" 改为 "SPDR"

### 4. 访客统计
- 添加不蒜子（Busuanzi）访客计数器
- 显示页面浏览量和独立访客数
- 双语支持，样式与页面一致

### 5. SEO 优化（2026-07-08）
- 添加 meta description、keywords、author
- 添加 Open Graph 标签（og:title/description/type/url/image/locale）
- 添加 Schema.org 结构化数据（Person 类型），帮助搜索引擎识别学者主页
- 添加 sitemap.xml 和 robots.txt，引导搜索引擎抓取

### 6. 论文与经历更新（2026-07-08）
- 论文数从 17 篇改为 18 篇
- PRL 添加注释 "Phys. Rev. Lett.(8篇)"（中英文均已添加）
- 教育经历中删除国名（日本、加拿大），中英文均已处理
  - 日本国立理化学研究所 → 国立理化学研究所
  - 日本东京大学 → 东京大学（出现两处）
  - 加拿大不列颠哥伦比亚大学 → 不列颠哥伦比亚大学
- 清华大学专业英文从 "Mathematical Sciences" 改为 "Physics"
- 页脚年份从 © 2024 改为 © 2026

---

## Git 工作流

### 提交规范
提交信息使用英文，简明描述变更内容。

### 推送流程
由于网络环境限制，推送时需要配置代理：

```bash
# 配置代理
git config --local http.proxy http://127.0.0.1:7890
git config --local https.proxy http://127.0.0.1:7890

# 推送（使用 HTTP/1.1 避免 HTTP2 错误）
git -c http.version=HTTP/1.1 push

# 清理代理配置
git config --local --unset http.proxy
git config --local --unset https.proxy
```

### GitHub 认证
- 使用 Personal Access Token (PAT) 认证，已存入 macOS 钥匙串
- 全局凭证助手已配置：`git config --global credential.helper osxkeychain`
- 推送时直接 `git push` 即可，无需手动输入用户名密码

### 注意事项
- VPN 端口为 7890
- 推送时可能遇到 HTTP2 framing layer 错误，使用 `-c http.version=HTTP/1.1` 解决
- 遇到 ref lock 错误时，先 `git pull --rebase` 再推送

---

## 未来修改指南

### 添加新内容
1. 在 `index.html` 对应区块中添加 HTML
2. 必须同时添加 `data-zh` 和 `data-en` 属性
3. 默认文本设为中文
4. 如需新样式，在 `style.css` 中添加

### 修改样式
1. 编辑 `style.css`
2. 更新 `index.html` 中 CSS 链接的版本号（`?v=xxx`）
3. 提醒用户在浏览器中硬刷新（Cmd+Shift+R）以清除缓存

### 添加新区块
1. 在 `<main>` 中添加 `<section id="xxx">`
2. 在 `<nav>` 中添加对应的导航链接
3. 在 `style.css` 中添加区块样式
4. 确保双语属性完整

### 修改照片
- 替换 `assets/photo.jpg`
- 照片理想比例约为 5:6（当前为 248×298）
- 椭圆镜框会裁剪照片，注意人物位置居中

---

## 已知限制

1. **无响应式设计**：CSS 中没有 media query，在移动端可能显示不佳
2. **无导航高亮**：当前导航链接没有 active 状态
3. **无平滑滚动**：锚点跳转无动画效果
4. **页脚年份硬编码**：© 2026 需要手动更新
5. **不蒜子依赖外部服务**：如果 busuanzi.ibruce.info 不可用，计数器会显示空白

---

## 重要提醒

1. **双语规则**：任何内容修改必须同时更新中文和英文版本
2. **缓存问题**：CSS 修改后必须更新版本号，否则浏览器可能使用旧版本
3. **推送代理**：推送 GitHub 需要 VPN 代理（端口 7890）
4. **不要使用 HTTP2**：推送时使用 `-c http.version=HTTP/1.1` 参数

---

## AI 接手指南

本文档是本项目的唯一知识源。新 AI 会话接手时，直接读取此文件即可获得完整上下文。

### 必须遵守的硬规则

1. **双语一致性**：修改任何文本时，必须同时更新 `data-zh`、`data-en` 属性以及元素内的默认文本（三处都要改）
2. **CSS 缓存版本号**：修改 `style.css` 后，必须更新 `index.html` 中 `?v=` 的值
3. **推送流程**：使用代理 + HTTP/1.1，详见「Git 工作流」章节
4. **提交规范**：commit message 使用英文
5. **记录归档**：所有项目记录、修改日志、操作指南统一写入本文件（`TECHNICAL_DOC.md`），本文件是唯一知识源

### 架构决策

- 双语实现：`data-zh` / `data-en` 属性 + JS 切换，无需 i18n 框架（单页应用够用）
- 访客统计：不蒜子（busuanzi.ibruce.info），国内可访问的免费服务
- 视觉风格：暖米色背景 `#faf8f5`，棕色系 `#6b5b3e` / `#8b7355`，Georgia 衬线字体

### 项目结构

纯静态文件，无构建步骤，无依赖：
- `index.html` — 所有内容 + 内联 JS
- `style.css` — 所有样式
- `assets/photo.jpg` — 个人照片（248×298，约 5:6）
- `TECHNICAL_DOC.md` — 本技术文档
