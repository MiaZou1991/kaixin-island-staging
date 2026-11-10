# KAIXIN 开心岛 · 独立 V2 测试环境

> **这不是正式开心岛，也不包含家里的真实作业或个人资料。** 专门用于一个家庭、一个孩子的 V2 登录、设备配对和跨设备同步验收。

- 正式环境：`MiaZou1991/homework-island-demo`，GitHub Pages 原站保持不动。
- **本测试仓库**：`MiaZou1991/kaixin-island-staging`。
- 预计测试网址（只有 Github Pages 部署成功才有效）：https://miazou1991.github.io/kaixin-island-staging/
- 后端：**仅** Supabase staging `tgnznizqhheiwgdhvwdc`，使用模拟数据；不连接生产 `syqcpfjudjniripzcjdq`。
- **源代码锁定**：单独在原仓库 Draft [PR #33](https://github.com/MiaZou1991/homework-island-demo/pull/33) 验收过的 staging 源码 commit `d1154b0d352d57d440685182024c0d6c70978b5d`；不会随原仓库 `main` 的更新自动覆盖测试站。
- 当前保留原版完整 Logo、Tab、高清素材、儿童端及家长端 UI，并在构建过程中移除旧家庭码同步脚本。
- **当前能力限制**：家长侧操作的 CAS/读回/本机队列已有测试；儿童端尚未获准向云端写入（含完成/撤销作业），故这阶段不代表 V2 功能最终验收通过。

## 如何部署测试站（一次性）

此仓库的 [自动部署工作流](.github/workflows/deploy-kaixin-staging.yml) 会在 `main` 更新后从固定源 SHA 构建独立测试包，自动发布到**本仓库自己的** GitHub Pages；不会提交任何文件到正式仓库。

**仓库所有者需要做一次设置**：

1. 在本仓库打开 [Settings → Pages](https://github.com/MiaZou1991/kaixin-island-staging/settings/pages)。
2. 在 **Build and deployment → Source** 选 **GitHub Actions**；不必新建建议的模板工作流，已有本仓库自己的发布工作流。
3. 保存后在本仓库 [Actions](https://github.com/MiaZou1991/kaixin-island-staging/actions) 检查部署；先前因未启用 Pages 而失败的部署可以重试，或在工作流页面点击 **Run workflow**（选择 `main`）。

如果要通过邮箱注册，**还需**在 Supabase 的**staging** 项目 [Authentication → URL Configuration](https://supabase.com/dashboard/project/tgnznizqhheiwgdhvwdc/auth/url-configuration) 设置允许跳转地址：

```text
https://miazou1991.github.io/kaixin-island-staging/index.html
```

先完成独立 HTTPS Pages 部署并验证地址再设置回跳。**不要**更改正式 Supabase 项目 URL 配置或其密钥。家长账号必须通过验证邮箱且由可信管理员授权；注册成功本身不等于取得家长权限。不要在聊天发送邮箱密码、验证码、配对码或 JWT。

## 研发与隔离红线

- 绝不将个人数据、现有 Safari 本地存储、生产 JSON 备份上传至公开仓库、CI 或 staging 数据库。
- 测试代码升级只应改变源 PR 验证过的固定 SHA，并通过 PR 检查；新仓库发布只影响 `/kaixin-island-staging/`。
- 不操作原仓库 `main` 和正式 Supabase；正式迁移需用户**单独确认**并进行完整备份与双 iPhone 复核。
- 页面虽公开可访问，但后端数据靠 Supabase Auth 身份和 RLS 严格保护；不得把公开页面当作隐私加密措施。
- 当前功能未完善前，不能把儿童端点完成的结果当作跨设备同步成功。

## 本仓库本地检查

GitHub Actions 中的 `verify` 任务会：
- 检查指定源 commit 的 V2 合并内核；
- 生成 `upstream/build/kaixin-staging` 的完整静态站；
- 拒绝生产 Supabase 项目标识、旧家庭码、旧实时通知脚本进入测试包；
- 仅在 `main` 允许触发测试 Pages 部署。

测试站使用 GitHub Pages 免费公开静态托管，**不是身份数据库**。
