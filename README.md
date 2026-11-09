# 作品集 · 维护说明

这个作品集分成两块：**网页版**（可上传成链接）和 **PDF 版**（投递附件）。
两者都从同一份清单 `works.json` 生成，所以加作品只改一个地方，网页和 PDF 一起更新。

## 目录

```
作品集/
├─ works.json          ← 唯一需要手改的文件（作品清单）
├─ portfolio.html      ← 由脚本生成，不要手改
├─ 程佳申-作品集.pdf     ← 由脚本生成，不要手改
├─ README.md
└─ images/
   ├─ 3d/     3D 作品图（png / jpg）
   ├─ vis/    平面 · VIS 作品图
   ├─ ps/     图像处理作品图
   ├─ ae/     动效作品（mp4 视频 + 可选封面图）
   └─ ui/     界面设计作品图
```

## 加一件新作品（三步）

1. 把图片放进对应类别的文件夹，命名带序号，例如 `images/ui/01_login.png`、`images/ps/01_poster.png`。
   AE 视频放 `images/ae/ae-06_xxx.mp4`，同时需要一张同名 `.jpg` 当封面帧（PDF 里用这张），
   例如 `ae-06_xxx.mp4` + `ae-06_xxx.jpg`。
2. 打开 `works.json`，在 `works` 数组末尾追加一条：

```json
{
  "cat": "ui",
  "kind": "image",
  "file": "images/ui/01_login.png",
  "title": "App 登录页",
  "meta": "界面设计 ｜ Photoshop ｜ 手机端",
  "desc": "一句话说明这张图在做什么、自己负责哪部分。",
  "note": "制作说明：耗时 / 卡点"
}
```

   视频条目多两个字段：

```json
{
  "cat": "ae",
  "kind": "video",
  "file": "images/ae/ae-06_xxx.mp4",
  "poster": "images/ae/ae-06_xxx.jpg",
  "title": "标题",
  "meta": "动效 ｜ After Effects ｜ 1920×1080 ｜ 5.0s",
  "desc": "画面里发生了什么。"
}
```

   - `cat` 必须是 `categories` 里已有的 key（`3d` / `vis` / `ps` / `ae` / `ui`）。
   - `kind` 写 `"image"` 或 `"video"`（视频才会在网页里播放）。
   - 条目按 `works` 里的先后顺序排列；同一个 `cat` 的条目会自动归到那一节。
3. 运行两个脚本：

```
python 作品集素材\build_site.py     # 重新生成 portfolio.html
python 作品集素材\build_pdf.py      # 重新生成 PDF
```

## 想改的地方

- **改抬头、简介、联系方式**：`works.json` 里的 `profile`。
- **改类别名称 / 说明 / 顺序**：`categories` 数组的顺序就是页面顺序。
  `theme` 为 `"light"` 时该类别用浅色版式（现在 VIS 用它）。
  `pdf_layout` 为 `"row3"` 是每页 3 件横排，`"quad"` 是每页 2×2。
- **空类别**：某类别还没有作品时，网页和 PDF 都会自动跳过它；加进第一条作品后它就会出现。
- **PDF 另存位置**：`build_pdf.py` 里的 `OUT`。

## 上传成链接（GitHub Pages）

现在已经发布好了：**https://chengjiashen-129.github.io/portfolio/**

更新时只需要动这一处：

1. 改完 `works.json`，跑 `build_site.py` 重新生成 `portfolio.html`。
2. 把 `portfolio.html` 复制一份改名 `index.html`（GitHub Pages 只认 `index.html` 当首页，
   少了它就会把你的 README 当首页显示，这点很容易踩坑）。
3. 去仓库页面上传：根目录传 `index.html`（和 `works.json`，如果改过），
   新增的图片按类别传到 `images/3d` / `vis` / `ps` / `ae` / `ui` 对应目录。
   上传页地址形如 `https://github.com/chengjiashen-129/portfolio/upload/main/images/ui`，
   选好文件后点页面底部的 **Commit changes** 才算真正提交。
4. 等 1–2 分钟 Pages 自动重建，打开链接确认新作品已出现。
5. 记得把 PDF 也重新生成一份，投递时一起给。

> 注意：网页版是通过 `document.querySelector` 做分类筛选的，纯本地双击也能用；上传后同样有效。
