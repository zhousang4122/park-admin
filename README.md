# 园区后台管理系统（月卡管理模块）

南昌大学 Web 前端开发实验项目 —— 使用 **原生 HTML + CSS + JavaScript** 开发的后台管理系统，不使用 Vue / React 等任何框架，不引入第三方 UI 库。

## 项目结构

```text
park-admin/
├── index.html        工作台首页：4 个统计卡片 + 快捷入口 + 扩展占位容器
├── monthCard.html    月卡管理列表页：查询筛选 / 分页 / 全选批量删除 /
│                     查看-编辑-续费弹窗 / 单条删除
└── addMonthCard.html 增加月卡页：新增 / 编辑复用（URL 参数 id 区分），
                     必填与正则校验、剩余天数与状态自动计算
```

三个页面均为自包含文件（CSS / JS 内嵌），放进同一个文件夹即可运行。

## 运行方法

- 方式一：直接用浏览器打开 `index.html`（推荐 Chrome）。
- 方式二：VS Code 安装 Live Server 插件，右键 `index.html` 选择 Open with Live Server。

## 功能清单

1. 后台统一布局：左侧侧边导航（折叠 / 子菜单展开 / 菜单高亮）+ 顶部头部（登录账号）。
2. 首页 4 项统计卡片：年度累计收费 / 入驻企业总数 / 一体杆总数（模拟固定值，金额千分位格式化）+ 月卡车辆总数（动态读取数组长度）。
3. 月卡列表：关键字 + 状态下拉查询、重置、前端分页（slice 切片）、每页条数切换、表头全选、批量删除。
4. 表格操作：查看（只读弹窗）、编辑（全部字段可改）、续费（复用编辑弹窗，车辆信息只读）、单条删除（confirm 二次确认）。
5. 增加月卡：URL 参数 `id` 区分新增 / 编辑模式，编辑自动回填；车牌与手机号正则校验、金额正数校验、日期先后校验，错误文字提示；剩余有效天数与状态（可用 / 已过期）由结束日期自动计算。
6. 数据持久化：所有数据存于浏览器 `localStorage`，存储 key 为 `monthCardData`，三个页面共用同一数据源；页面刷新数据不丢失，首页统计与列表数据自动联动。

## 数据说明

- 首次打开自动写入 6 条默认模拟数据。
- 数据保存在浏览器本地（localStorage），清除浏览器缓存或无痕模式会重置；本实验按教学要求使用 localStorage 模拟后端存储。
- 月卡字段：`id / plate(车牌) / ownerName(车主) / phone(手机号) / payAmount(缴费金额) / startDate / endDate / remainDay(剩余天数) / status(0可用 1已过期) / remark(备注)`。

## 部署到公网（GitHub + 阿里云 ECS + Nginx）

本项目是纯静态页面，用 Nginx 托管即可。简要步骤：

1. 推送本仓库到 GitHub。
2. 阿里云购买一台 ECS（选 Ubuntu 24.04 或 CentOS，安全组放行 80 / 443 / 22 端口）。
3. 服务器安装 Nginx：`sudo apt update && sudo apt install -y nginx`。
4. 服务器克隆仓库：`sudo apt install -y git && git clone <你的仓库地址>`。
5. 把 `park-admin` 目录发布到 Nginx 站点根目录：`sudo cp -r park-admin/* /var/www/html/`。
6. 浏览器访问 `http://服务器公网IP/` 验证，完成。
