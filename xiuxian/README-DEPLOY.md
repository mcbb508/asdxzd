# 自动化部署指南 - GitHub Actions -> ClawCloud Run

本指南将帮助您配置 GitHub Actions，实现代码推送到 GitHub 后自动构建并部署到 ClawCloud。

## 1. 准备工作

1.  **ClawCloud 账号**: 确保您有 ClawCloud 账号并开通了容器镜像服务 (CR) 和 Cloud Run 服务。
2.  **GitHub 仓库**: 将您的代码上传到 GitHub 仓库。

## 2. 获取 ClawCloud 凭证

登录 ClawCloud 控制台，找到 **容器镜像服务 (Container Registry)**，获取以下信息：
- **仓库地址 (Registry URL)**: 通常是 `registry.clawcloud.cn`
- **命名空间 (Namespace)**: 您创建的组织或个人空间名称
- **用户名和密码**: 用于登录镜像仓库的凭证

## 3. 配置 GitHub Secrets

为了让 GitHub Actions 能够访问您的 ClawCloud 账号，需要在 GitHub 仓库中设置密钥。

1.  进入您的 GitHub 仓库页面。
2.  点击 **Settings** -> **Secrets and variables** -> **Actions**。
3.  点击 **New repository secret**，依次添加以下密钥：

| Name | Value (示例) | 说明 |
|------|--------------|------|
| `CLAW_REGISTRY_URL` | `registry.clawcloud.cn` | 镜像仓库地址 |
| `CLAW_REGISTRY_USERNAME` | `your-username` | 镜像仓库登录用户名 |
| `CLAW_REGISTRY_PASSWORD` | `your-password` | 镜像仓库登录密码 |
| `CLAW_NAMESPACE` | `your-namespace` | 您的命名空间名称 |

## 4. 首次部署与自动更新设置

### 步骤 A: 触发首次构建
1.  将包含 `.github/workflows/deploy.yml` 的代码推送到 GitHub `main` 或 `master` 分支。
2.  在 GitHub 仓库的 **Actions** 标签页中，您应该能看到 `Build and Deploy to ClawCloud` 工作流正在运行。
3.  等待工作流显示绿色对勾（成功），这意味着镜像已推送到 ClawCloud。

### 步骤 B: 在 ClawCloud 创建服务
1.  登录 ClawCloud Run 控制台。
2.  **创建服务**:
    - 选择 **从镜像部署**。
    - 镜像地址填写: `registry.clawcloud.cn/<您的命名空间>/xiuxian-game:latest` (请根据实际情况替换)。
    - **容器端口**: 设置为 `80`。
3.  **开启自动部署 (如果支持)**:
    - 在服务设置中，查看是否有“自动部署”或“触发器”选项。如果有，启用它，这样当新镜像 (`latest`) 推送时，服务会自动更新。
    - 如果没有自动部署选项，您可能需要在每次推送代码后，手动在控制台点击“重新部署”或“更新修订版本”。

## 5. 访问网站

部署成功后，ClawCloud 会为您分配一个公网域名（例如 `https://xiuxian-game-xxxxx.claw.run`）。点击即可访问您的修仙游戏！

---

### 故障排除

- **GitHub Action 失败**: 检查 Secrets 是否填写正确，特别是密码和命名空间。
- **服务无法访问**: 检查 ClawCloud Run 服务配置中的端口是否为 `80`。
