# 部署指南 - ClawCloud Run

本项目已配置为 Docker 容器化部署。以下是将您的 Web 应用打包并部署到 ClawCloud Run 的步骤。

## 1. 前置准备

确保您已安装以下工具：
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (用于本地构建)
- ClawCloud 命令行工具 (如果可用) 或访问 ClawCloud 控制台

## 2. 本地构建与测试 (可选)

在部署之前，建议在本地测试镜像：

```bash
# 构建镜像
docker build -t my-xiuxian-game .

# 运行容器
docker run -d -p 8080:80 my-xiuxian-game
```

访问 `http://localhost:8080` 确认游戏可以正常运行。

## 3. 部署到 ClawCloud Run

### 方法 A: 使用控制台上传 (最简单)

1.  **构建镜像**: 在本地执行 `docker build -t my-xiuxian-game .`
2.  **导出镜像**: 将镜像保存为文件 `docker save -o my-xiuxian-game.tar my-xiuxian-game`
3.  登录 ClawCloud 控制台。
4.  进入 **容器镜像服务 (CR)** 或直接进入 **Cloud Run** 服务。
5.  上传您的镜像或连接您的 GitHub/GitLab 仓库进行自动构建。
6.  创建一个新的 Service，选择刚刚上传的镜像。
7.  **端口设置**: 确保容器端口设置为 `80`。

### 方法 B: 使用 Docker Push (推荐)

如果您有 ClawCloud 的镜像仓库地址 (Registry URL)：

1.  **登录仓库**:
    ```bash
    docker login registry.clawcloud.cn
    # 输入您的用户名和密码
    ```

2.  **标记镜像**:
    ```bash
    # 将 <namespace> 替换为您的命名空间
    docker tag my-xiuxian-game registry.clawcloud.cn/<namespace>/xiuxian-game:v1
    ```

3.  **推送镜像**:
    ```bash
    docker push registry.clawcloud.cn/<namespace>/xiuxian-game:v1
    ```

4.  在 ClawCloud Run 控制台中创建服务，并选择 `registry.clawcloud.cn/<namespace>/xiuxian-game:v1` 镜像。

## 4. 验证

部署完成后，ClawCloud 会提供一个公网访问地址。访问该地址即可开始游戏。
