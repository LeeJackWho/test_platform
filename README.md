# 巧克力测试平台（test_platform）

> **说明**：本仓库为个人作品，因历史原因以 fork 形式存在于当前账号下，代码与内容均由本人（LeeJackWho）独立开发与维护，早期版本发布于个人旧账号 ljxpython。

基于 Ant Design Pro（UmiJS Max）搭建的测试管理与压测一体化平台前端，提供接口测试用例的集中管理、测试套件编排、定时计划执行与结果留痕，以及 Locust 压测场景的运行、结果查看与监控看板接入能力。

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![language](https://img.shields.io/badge/language-TypeScript-3178C6.svg)](https://www.typescriptlang.org/)
[![framework](https://img.shields.io/badge/framework-React-18-61DAFB.svg)](https://react.dev/)
[![ui](https://img.shields.io/badge/UI-Ant%20Design-1677FF.svg)](https://ant.design/)
[![build](https://img.shields.io/badge/build-UmiJS%20Max-877AEB.svg)](https://umijs.org/docs/max)

## 核心功能

### 项目管理
- 测试项目的列表查询、新增、编辑、删除与详情查看，项目作为测试资产的归属维度。

### 接口测试
- **测试模块**：按模块维度组织接口，支持从服务端同步模块数据。
- **测试用例**：用例列表查询、按模块/场景筛选，支持从服务端同步用例。
- **测试套件**：从已有用例挑选组成套件，支持创建、按用例 ID 重新同步、运行。
- **测试计划**：为套件配置执行时间或Cron 表达式定时执行，支持计划的增删改查。
- **测试结果**：按计划/套件执行后落库的结果列表与结果详情（含运行时间、耗时等字段）。

### 压测（Locust）
- **压测 Case**：Locust 压测用例的列表查询、同步与删除。
- **压测套件**：从压测 Case 组装套件，支持创建与按 Case ID 同步更新。
- **压测运行**：检查 Locust 进程状态、启动/停止/强制停止压测任务，并在页面内嵌 Locust Web 界面。
- **压测结果**：压测结果列表与结果详情。
- **监控看板**：预留看板接入位，以告警条 + 看板视图的形式按业务需要嵌入。

### 平台能力
- 登录 / 登出与全局布局（ProLayout），基于 `initialState` 的权限控制（`access.ts` 中的 `canAdmin` / `canTest`）。
- 关键列表页内置「使用建议」公告提示，可关闭。
- 统一请求与错误处理（`src/requestErrorConfig.ts`），按业务错误码分级提示。

## 技术栈

| 类别 | 选型 |
| --- | --- |
| 框架 | UmiJS Max 4（`@umijs/max`）+ React 18 + TypeScript 5 |
| UI | Ant Design 5、@ant-design/pro-components、@ant-design/icons、antd-style |
| 路由/数据流 | Umi 约定式路由（`config/routes.ts`）、layout / model / initialState / access / request 插件 |
| 网络请求 | `@umijs/max` 的 `request`（axios）+ ahooks useRequest |
| 国际化 | Umi 国际化插件，`zh-CN` 为默认语言，内置 8 种语言包 |
| 服务端 | Express（`server.js`，仅用于本地静态托管与 `/api` 反向代理） |
| 构建 | Umi Max（`max build`）、esbuild 压缩、MFSU |
| 质量工具 | ESLint、Prettier、husky + lint-staged、`tsc --noEmit` |
| 测试 | Jest 29 + @testing-library/react + jsdom（`jest.config.ts`） |
| 包管理 | pnpm（仓库含 `pnpm-lock.yaml`），npm 亦可 |

## 目录结构

```
test_platform
├── config/                 # Umi 配置
│   ├── config.ts           # 主配置：布局、国际化、request、access、openAPI 等
│   ├── routes.ts           # 路由与菜单定义
│   ├── proxy.ts            # 本地开发代理（/api -> 后端服务）
│   ├── defaultSettings.ts  # ProLayout 默认设置
│   └── oneapi.json         # openAPI schema
├── mock/                   # 本地 mock 数据（dev 默认 MOCK=none，不生效）
├── public/                 # 静态资源（logo、monitor.png 等）
├── src/
│   ├── components/         # 全局组件（Header、Footer、RightContent 等）
│   ├── locales/            # 国际化语言包
│   ├── models/             # 全局数据流
│   ├── pages/
│   │   ├── Project/        # 项目管理：列表、创建、详情
│   │   ├── openapitest/    # 接口测试：模块、用例、套件、计划、结果
│   │   ├── LocustTest/     # 压测：case、suite、run、result
│   │   ├── User/Login/     # 登录
│   │   └── ...             # Welcome / Goods / TableList 等脚手架示例页
│   ├── services/           # 后端接口封装（按业务模块分目录）
│   │   ├── test_project/ test_moudle/ test_case/
│   │   ├── test_suite/ test_plan/ test_run/
│   │   └── locust_case/ locust_suite/ locust_run/ locust_result/
│   ├── access.ts           # 权限定义
│   ├── app.tsx             # 运行时配置与布局
│   └── requestErrorConfig.ts
├── tests/setupTests.jsx    # Jest 环境准备
├── types/                  # 全局类型与缓存类型
├── jest.config.ts
├── server.js               # Express 静态服务 + /api 代理
└── package.json
```

## 本地开发

环境要求：Node.js >= 12（`package.json` 的 `engines` 声明），pnpm 或 npm 均可。

```bash
# 安装依赖（推荐 pnpm，与仓库 lock 文件一致）
pnpm install

# 启动开发服务，默认 http://localhost:8000
pnpm start
```

常用脚本（均来自 `package.json`）：

| 命令 | 说明 |
| --- | --- |
| `pnpm start` | 以 `UMI_ENV=dev` 启动开发服务（走本地 mock） |
| `pnpm dev` | 以 `REACT_APP_ENV=dev`、`MOCK=none` 启动，连真实后端（联调推荐） |
| `pnpm start:no-mock` | 关闭 mock 启动 |
| `pnpm start:test` / `pnpm start:pre` | 以 test / pre 环境变量启动 |
| `pnpm build` | 生产构建，产物在 `dist` |
| `pnpm serve` | 用 `umi-serve` 启动本地静态服务 |
| `pnpm test` | 运行 Jest 测试 |
| `pnpm test:coverage` | 运行测试并输出覆盖率 |
| `pnpm lint` | 依次执行 ESLint、Prettier、tsc 检查 |
| `pnpm lint:fix` | ESLint 自动修复 |
| `pnpm tsc` | 仅做类型检查 |
| `pnpm openapi` | 依据 openAPI schema 生成 services 与 mock |

前后端联调时，需要本地后端服务运行在 `5001` 端口：`config/proxy.ts` 会把 `/api/` 代理到 `http://localhost:5001`。生产环境可使用 `node server.js` 托管 `dist` 静态资源（默认 `3000` 端口）并把 `/api` 转发到后端。

## 注意事项

1. **后端服务不在本仓库内**。所有业务数据依赖 `/api/...` 接口，本仓库只包含前端；本地开发前需自行启动后端服务并确保 `5001` 端口可访问，否则列表页会因请求失败而空数据。
2. **压测运行环境依赖外部服务**。压测运行页会内嵌 Locust Web 页面，其地址在 `src/pages/LocustTest/run/locustweb.tsx` 中硬编码，换环境时需修改该常量；`checkLocustProcess`、`stopLocustTest`、`forceStopLocustTest` 等能力也依赖后端实际管理 Locust 进程。
3. **监控看板为占位实现**。`src/pages/LocustTest/run/monitor.tsx` 仅提供告警提示与静态图片展示，看板需按业务形态自行嵌入。
4. **示例页可清理**。`src/pages/Goods`、`src/pages/TableList`、`mock/` 目录保留自 Ant Design Pro 脚手架，其中 `/goods` 已在路由中 `hideInMenu`，正式使用时可一并删除。
5. **提交前会自动检查**。仓库配置了 husky `pre-commit` 钩子执行 `lint-staged`（ESLint + Prettier），提交时请确保本地检查通过。

## License

[MIT](./LICENSE)