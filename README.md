# 童创星河 OA · 开发基座

> 基于芋道（yudaocode）开源快速开发平台搭建的 MVP 开发基座，用于承载《童创星河 OA · 功能开发需求清单 V2.0》的全部研发工作。

## 1. 这个仓库是什么

本仓库是童创星河 OA 系统的**技术底座**，不是成品业务系统。它在成熟开源框架的基座上，预留了业务开发所需的通用能力，团队在此之上实现清单中的 15 个业务模块。

采用「**可复用底座 + 自研业务内核**」策略：

- **底座（本仓库已备好）**：登录认证、RBAC 权限、菜单按钮、操作审计、字典、文件上传、消息通知、定时任务、代码生成器、工作流引擎。
- **内核（需团队自研）**：五项定额分账引擎、四层收益、双侧记账事务、课时四段式台账、推荐奖 / 理事分红、五类主体财务对账。这部分框架不提供，也是本项目的最大风险项。

## 2. 技术栈与来源

| 项 | 说明 |
| --- | --- |
| 基座来源 | 芋道 `yudao-boot-mini`（后端精简版）+ `yudao-ui-admin-vue3`（前端） |
| 开源协议 | MIT，个人与企业可免费商用 |
| 后端分支 | `master-jdk17`（JDK 17 + Spring Boot 3.x） |
| 后端框架 | Spring Boot + MyBatis Plus + Redis + Redisson + Spring Security |
| 前端框架 | Vue 3 + Element Plus + Vite + pnpm |
| 数据库 | MySQL（脚本已内置，另适配 PostgreSQL / Oracle / 达梦等） |
| 工作流 | Flowable（默认注释，按需启用） |
| 上游版本 | 后端 f03151e / 前端 c67f134（2026-10 发布） |

> 精简版默认仅启用 `yudao-module-system` 与 `yudao-module-infra` 两个模块，其余业务模块在 `backend/pom.xml` 中均以注释形式关闭，保持基座轻量。

## 3. 目录结构

```
tongchuang-oa-base/
├── backend/        # 后端（Spring Boot 多模块）
│   ├── yudao-dependencies/    # 依赖版本统一管理
│   ├── yudao-framework/       # 框架层（15+ 个 Starter）
│   ├── yudao-module-system/   # 系统功能：用户/角色/菜单/租户/字典/日志
│   ├── yudao-module-infra/    # 基础设施：代码生成/文件/配置/定时任务
│   ├── yudao-server/          # 启动模块（聚合业务模块，对外 REST API）
│   └── sql/mysql/             # 数据库初始化脚本
├── frontend/       # 前端（Vue3 + Element Plus 管理后台）
└── README.md
```

## 4. 本地启动

### 后端

```bash
# 1. 准备 MySQL（建议 8.0）与 Redis
# 2. 创建数据库 ruoyi-vue-pro，导入脚本：
#    backend/sql/mysql/ruoyi-vue-pro.sql
#    backend/sql/mysql/quartz.sql
# 3. 修改 backend/yudao-server/src/main/resources/application-local.yaml 中的 DB/Redis 连接
# 4. 编译并启动
cd backend
mvn clean install -DskipTests
# 启动主类：cn.iocoder.yudao.server.YudaoServerApplication
# 默认端口 48080，返回 {"code":401,"msg":"账号未登录"} 即启动成功
```

### 前端

```bash
cd frontend
pnpm install
pnpm run dev
# 默认 http://localhost:80，默认账号 admin / admin123
```

详细文档见芋道官方：<https://doc.iocoder.cn/quick-start/>

## 5. 二次开发指引（重要）

开发前请对照需求清单，明确哪些用现成的、哪些要自己写：

**可直接复用**：账号密码登录与错误锁定、令牌会话与强制下线、菜单按钮权限、角色管理、操作审计、字典与编号、文件上传、消息通知、定时任务、工作流审批。

**需改造**：
- 组织架构——清单要求「市场运营 / 交付中心」双分支组织树，框架默认是单部门树，需重构。
- 权限模型——清单要求一人多身份、按当前身份重建数据上下文、服务端强制按业务域裁剪数据（渠道侧不可见交付成本、交付侧不可见进货价）。框架 RBAC 需深度扩展，**不可依赖前端过滤**。

**必须自研（框架无对应能力）**：代理进货与课时批次库存、生源派单排课、陪练九档分级、五项定额分账引擎、四层收益、推荐 / 理事分红、工资提现、五类主体财务对账。

**特别提醒**：
1. 不要套用芋道商城 / 分销模块的佣金逻辑，与本项目的「定额管道 + 理事分红截断」是两套模型，硬套会算错账。
2. 分账引擎开发前，务必先完成清单要求的「分润算例对账」（218/170 拆解、130 元一单回本、五项之和校验），否则必然返工。
3. 交付记录是分润与工资的唯一计费依据，销售单不得直接驱动分润。

## 6. 数据来源

需求清单原件：童创星河OA_MVP_功能开发需求清单_V2.0.xlsx（15 模块 / 126 条 / 519.5 人天）。
