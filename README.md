# 童生童梦成长空间 · 官网（静态站点）

一个**纯静态**的单页官网，无需任何构建/编译、无需后端、无需数据库。
把整个文件夹原样上传到任意静态托管服务，即可形成一个可外网访问的网站。

---

## 一、目录结构

```
kids/
├─ index.html              # 网站主页面（入口文件）
├─ README.md               # 本说明文件
└─ assets/
   ├─ styles.css           # 全站样式
   ├─ favicon.ico          # 站点图标
   ├─ favicon-32.png
   ├─ favicon-512.png
   ├─ apple-touch-icon.png
   ├─ logo-icon.png        # 品牌 logo（图标）
   ├─ logo-full.png        # 品牌 logo（完整）
   └─ photos/              # 所有图片素材（校区、师资、证书、家长头像等）
```

> 入口文件是 `index.html`，部署后访问域名根路径即可打开。

---

## 二、本地预览

任选其一：

- **最简单**：双击 `index.html` 用浏览器打开。
- **推荐（更接近线上效果）**：在 `kids/` 目录下启动本地服务器
  ```bash
  # Python 3
  python3 -m http.server 8080
  # 然后浏览器访问 http://localhost:8080
  ```

---

## 三、部署到云服务（任选一种）

本站是静态网站，**只要把 `kids` 文件夹里的内容上传上去并把 `index.html` 设为首页/索引页即可**。

### 方案 0：连接 Git 仓库一键部署（推荐，自动持续部署）

本目录已是一个 Git 仓库，并已内置部署配置：
`vercel.json`（Vercel）、`netlify.toml`（Netlify）、`.github/workflows/deploy-pages.yml`（GitHub Pages）。

**第一步：推送到 GitHub（仅首次）**
```bash
# 在 GitHub 网页上新建一个空仓库（不要勾选 README），拿到仓库地址后：
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git branch -M main
git push -u origin main
```

**第二步：连接云服务（任选其一，点几下即可）**
- **Vercel**：[vercel.com/new](https://vercel.com/new) → Import 你的仓库 → Framework 选 "Other" → Deploy（`vercel.json` 已配好，无需改动）。
- **Netlify**：[app.netlify.com](https://app.netlify.com) → "Add new site" → "Import an existing project" → 选仓库 → 直接 Deploy（`netlify.toml` 已把发布目录设为根目录）。
- **GitHub Pages**：仓库 Settings → Pages → Source 选 **GitHub Actions** 即可；之后每次 `git push` 都会自动部署（工作流已内置）。

之后**每次 `git push`，网站都会自动重新部署**，无需再手动上传。

---

### 方案 A：国际服务，免备案、最快上线（适合测试 / 海外访问）

| 服务 | 操作 |
| --- | --- |
| **Netlify** | 登录 [app.netlify.com](https://app.netlify.com) → "Add new site" → "Deploy manually" → 把 `kids` 文件夹直接拖进去，几秒后得到 `xxx.netlify.app` 网址 |
| **Vercel** | 安装 `npm i -g vercel` → 在 `kids/` 目录运行 `vercel`，按提示即可 |
| **Cloudflare Pages** | dash.cloudflare.com → Workers & Pages → Create → Upload assets → 上传 `kids` 内容 |
| **GitHub Pages** | 把 `kids` 内容推到仓库 → Settings → Pages → 选择分支与根目录 → 得到 `用户名.github.io/仓库名` |

拖拽即可上线，无需服务器、无需备案，自动带 HTTPS。

### 方案 B：国内云服务（适合国内正式上线，访问速度快）

> 国内服务器/CDN 绑定**自有域名**需先完成 **ICP 备案**（页脚预留了"京ICP备XXXXXXXX号"，备案通过后替换为真实备案号）。

**阿里云 OSS 静态网站托管（示例）**
1. 开通对象存储 OSS，创建一个 Bucket（读写权限设为"公共读"）。
2. 把 `kids/` 里的 `index.html`、`assets/` 全部上传到 Bucket 根目录（保持目录结构）。
3. Bucket → 基础设置 → **静态页面** → 默认首页填 `index.html`，默认 404 页可填 `index.html`。
4. 用 OSS 的访问域名即可打开；正式上线建议在前面挂 **CDN + 自有域名 + HTTPS 证书**。

腾讯云 COS、华为云 OBS、又拍云、七牛云的操作思路完全一致：**上传文件 → 开启静态网站功能 → 指定 index.html 为首页 → 绑定域名**。

### 方案 C：自己的服务器（Nginx）

把 `kids/` 内容放到网站根目录（如 `/var/www/kids`），Nginx 配置示例：
```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/kids;
    index index.html;
    location / { try_files $uri $uri/ /index.html; }
}
```

---

## 四、上线前可替换的占位内容

- **电话**：`17801780530`（页面显示为 `178-0178-0530`）
- **邮箱**：`hello@tstm-kids.com`
- **地址**：北京市丰台区方庄购物中心三层
- **备案号**：`京ICP备XXXXXXXX号` → 替换为真实备案号
- **统计/客服**：如需接入百度统计、微信客服等，可在 `index.html` 的 `</body>` 前粘贴对应代码

---

## 五、可选优化（非必须）

`assets/photos/` 中的 `feat-*.png`、`skill-*.png` 为高清大图（单张约 2MB）。
若希望外网加载更快，可将这些 PNG 压缩或转为 JPG/WebP（体积可降至原来的 1/5 ~ 1/10）。
需要的话可以告诉我，我来批量优化并更新引用。
