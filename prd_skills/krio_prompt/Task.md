# 用户管理系统实施任务清单

说明：
- 任务按依赖顺序组织，可直接作为迭代执行清单。
- 每个任务控制在 2-6 小时粒度，含测试要求。
- 需求追踪使用 `REQ-xx` 标记。

- [ ] 1. 项目基础与工程骨架
- [ ] 1.1 初始化后端工程与目录结构
  - 创建 `api`, `domain`, `service`, `repository`, `infra`, `tests` 模块
  - 配置环境变量管理与配置加载
  - _Requirements: REQ-09_
- [ ] 1.2 接入数据库与迁移框架
  - 配置 PostgreSQL 连接、迁移脚手架
  - 创建基础迁移执行流程（dev/test）
  - _Requirements: REQ-09_
- [ ] 1.3 接入日志与 request_id 中间件
  - 输出结构化日志，统一 trace/request_id
  - _Requirements: REQ-09_

- [ ] 2. 数据模型与迁移
- [ ] 2.1 建立用户与会话相关表
  - 创建 `users`, `sessions_refresh_tokens` 表及索引
  - _Requirements: REQ-01, REQ-02, REQ-03, REQ-06_
- [ ] 2.2 建立 RBAC 数据表
  - 创建 `roles`, `permissions`, `role_permissions`, `user_roles` 表
  - 写入默认角色与权限种子数据
  - _Requirements: REQ-05_
- [ ] 2.3 建立审计日志表
  - 创建 `audit_logs` 表及查询索引
  - _Requirements: REQ-07_

- [ ] 3. 认证与会话模块
- [ ] 3.1 实现注册与激活服务
  - 注册、邮箱唯一校验、激活 token 生成与验证
  - _Requirements: REQ-01_
- [ ] 3.2 实现登录与锁定策略
  - 密码校验、失败计数、锁定解锁逻辑
  - _Requirements: REQ-02, REQ-06_
- [ ] 3.3 实现刷新令牌轮换与登出
  - 刷新令牌持久化（哈希）、轮换、撤销
  - _Requirements: REQ-02_
- [ ] 3.4 实现密码重置流程
  - forgot/reset 两段式流程与过期策略
  - _Requirements: REQ-06_
- [ ] 3.5 实现 MFA 验证流程（可配置）
  - 接入一次性验证码校验与登录二步流程
  - _Requirements: REQ-06_

- [ ] 4. 用户资料与后台用户管理
- [ ] 4.1 实现个人资料查询与更新
  - `GET/PATCH /users/me`，字段级校验与保留字段保护
  - _Requirements: REQ-03_
- [ ] 4.2 实现后台用户查询与分页过滤
  - 支持邮箱、状态、角色过滤
  - _Requirements: REQ-04_
- [ ] 4.3 实现后台用户创建与状态变更
  - 管理员创建、禁用、启用用户
  - _Requirements: REQ-04_
- [ ] 4.4 实现后台重置用户密码
  - 管理员重置密码并使旧凭据失效
  - _Requirements: REQ-04, REQ-06_

- [ ] 5. RBAC 权限系统
- [ ] 5.1 实现角色 CRUD
  - 创建、编辑、删除角色及删除前引用检查
  - _Requirements: REQ-05_
- [ ] 5.2 实现权限点与角色绑定
  - 角色权限分配、回收与查询
  - _Requirements: REQ-05_
- [ ] 5.3 实现用户角色分配接口
  - 用户分配/移除角色，权限变更即时生效
  - _Requirements: REQ-05_
- [ ] 5.4 实现统一鉴权中间件
  - 基于权限码控制接口访问，标准化 401/403
  - _Requirements: REQ-05_

- [ ] 6. 审计与通知
- [ ] 6.1 实现审计事件写入组件
  - 登录失败、锁定、密码重置、角色变更等事件统一写入
  - _Requirements: REQ-07_
- [ ] 6.2 实现审计日志查询接口
  - 多条件过滤、分页、审计员只读访问
  - _Requirements: REQ-07_
- [ ] 6.3 实现通知任务与重试机制
  - 注册激活、密码重置、异常登录通知
  - _Requirements: REQ-08_

- [ ] 7. 安全与可靠性增强
- [ ] 7.1 实现登录与关键接口限流
  - IP + 账号双维限流与 429 返回
  - _Requirements: REQ-06_
- [ ] 7.2 增强输入校验与敏感信息脱敏
  - 参数校验、错误信息规范、日志脱敏
  - _Requirements: REQ-06, REQ-09_
- [ ] 7.3 实现健康检查与运维指标
  - `/health/live` `/health/ready`，核心指标暴露
  - _Requirements: REQ-09_

- [ ] 8. 测试与验收
- [ ] 8.1 编写单元测试
  - 覆盖密码规则、锁定策略、权限决策、状态迁移
  - _Requirements: REQ-01, REQ-02, REQ-05, REQ-06_
- [ ] 8.2 编写集成测试
  - 覆盖认证、用户管理、RBAC、审计主要接口
  - _Requirements: REQ-01 ~ REQ-07_
- [ ] 8.3 编写端到端测试
  - 核心业务流：注册激活->登录->资料更新->管理员禁用->登录失败
  - _Requirements: REQ-01 ~ REQ-07_
- [ ] 8.4 非功能验证
  - P95 性能、限流策略、告警与日志可观测性验证
  - _Requirements: REQ-08, REQ-09_

- [ ] 9. 发布准备
- [ ] 9.1 产出部署与回滚说明
  - 数据库迁移顺序、配置项说明、回滚策略
  - _Requirements: REQ-09_
- [ ] 9.2 产出管理员操作手册
  - 用户管理、角色权限、审计查询使用说明
  - _Requirements: REQ-04, REQ-05, REQ-07_

## 里程碑建议
1. M1（基础可运行）：完成 1 + 2 + 3.1~3.3。
2. M2（后台可用）：完成 4 + 5。
3. M3（可审计可运维）：完成 6 + 7。
4. M4（可上线）：完成 8 + 9。
