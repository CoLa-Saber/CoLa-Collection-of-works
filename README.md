# 陈雨可 | 设计师个人作品集

一个纯静态的个人作品集网站，主交付文件为单文件 HTML（所有样式与脚本内联），双击即可打开，无需任何构建工具。

## 线上访问地址

| 项目 | 值 |
| --- | --- |
| 正式网址 | https://shenzhen-d9g9yepss24fd7e4a-1486582819.tcloudbaseapp.com/ |
| 托管方式 | CloudBase 静态网站托管（CDN 加速） |
| 云环境 ID | `shenzhen-d9g9yepss24fd7e4a` |
| 部署时间 | 2026-09-12 |

> 该网址固定不变。后续内容更新重新部署后，链接保持不变，CDN 通常几分钟内刷新。

## 文件结构

```
├── index.html              # 线上站点首页（与 陈雨可-作品集.html 内容一致）
├── 陈雨可-作品集.html        # 单文件交付版，可离线双击打开
├── cloudbase-sdk.js        # 预打包的 CloudBase Web SDK（浏览器可直接引用）
├── favicon.svg
├── public/                 # 静态资源源目录
│   ├── images/             # 作品图片（40 个）
│   │   └── portfolio/      # 各作品详情页幻灯片
│   └── videos/hero-bg.mp4  # 首屏人物视频
└── README.md
```

## 页面结构

| 区块 | 锚点 | 说明 |
| --- | --- | --- |
| 首屏 | `#home` | 杂志式左文右图 Hero，含描边年份水印、星座连线、数据卡 |
| 作品 | `#portfolio` | 项目卡片网格 + 详情弹窗（图片轮播） |
| 简历 | `#resume` | 档案卡、教育背景、经历、能力标签 |
| 互动 | `#interact` | 访问统计 + 留言板（数据存于云端） |
| 联系 | `#contact` | 邮件与微信/电话入口 |

## 内容编辑索引

| 想改什么 | 搜索关键词 |
| --- | --- |
| 姓名 / 职位标签 | `resume-name` / `resume-titles` |
| 简历简介 | `resume-tagline` |
| 项目数据 | `const PROJECTS` |
| 统计数据 | `class="stat-number"` |
| 年份水印 / 星座标签 / 标语 | `hero-emblem` |
| 留言板文案 | `iv-panel-note` |
| 底部版权 | `class="footer"` |

## 模块注释索引（HTML 内）

- `全站暗纹背景` — 全站背景纹理
- `HERO 模块` — 首屏
- `1. 人物光晕` / `2. 背景网格` / `3. 轨道虚线圆` / `4. 散布光点` / `5. 准星 + 坐标`
- `6.5 巨型描边水印` — 年份水印徽标
- `6. 暗角` / `7. 右侧书脊描边字` / `8. 悬浮玻璃数据卡` / `9. 底部标识文字`
- `简历档案模块（RESUME）`
- `INTERACT 互动区` — 访问统计 + 留言板

## 云端数据（互动区）

互动区通过 CloudBase 直连数据库，前端使用 `@cloudbase/js-sdk`（已预打包为 `cloudbase-sdk.js`），**不使用任何云函数**。

### 数据表

| 表名 | 用途 | 权限 |
| --- | --- | --- |
| `guestbook_messages` | 留言内容 | 任何人可读可见留言、可新增；不可修改或删除 |
| `site_visits` | 访问记录 | 仅可新增；原始记录不对外读取 |

访问统计通过数据库函数 `get_visit_stats()` 返回汇总数字（累计 / 今日 / 近七日 / 留言数），只暴露计数，不暴露原始访问明细。

### 相关配置

- 匿名登录：已开启（访客无需注册即可留言）
- 安全域名：`shenzhen-d9g9yepss24fd7e4a-1486582819.tcloudbaseapp.com`（已加入白名单）
- SDK 打包命令（如需更新）：
  ```bash
  cd sdk-build
  npm install @cloudbase/js-sdk esbuild
  ./node_modules/.bin/esbuild entry.js --bundle --format=iife \
    --platform=browser --target=es2018 --minify --outfile=cloudbase-sdk.js
  ```

## 重新部署

```bash
# 1. 确认登录与环境
tcb login
tcb env use shenzhen-d9g9yepss24fd7e4a

# 2. 把最新页面同步为首页
cp 陈雨可-作品集.html index.html

# 3. 上传（覆盖同名文件）
tcb hosting deploy . --env-id shenzhen-d9g9yepss24fd7e4a --yes

# 4. 验证
tcb hosting list --env-id shenzhen-d9g9yepss24fd7e4a
```

## 本地预览

留言与访问统计依赖云端接口，需通过 HTTP 协议访问（`file://` 会被浏览器跨域策略拦截）：

```bash
cd public && python3 -m http.server 8899
# 打开 http://localhost:8899/陈雨可-作品集.html
```
