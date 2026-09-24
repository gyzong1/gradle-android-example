# Gradle (Android) + JFrog CLI GitHub Actions 示例

基于测试项目 [gyzong1/gradle-android-example](https://github.com/gyzong1/gradle-android-example)，用 JFrog CLI 完成：

1. **构建并上传**至 Artifactory
2. **搜集并发布** Build Info
3. **扫描** Build（Xray）

示例工作流文件：`.github/workflows/gradle.yml`

## 使用方法

将 `gradle.yml` 复制到目标仓库：

```text
.github/workflows/gradle.yml
```

### Github 仓库配置


| 类型       | 名称                | 说明                                           |
| -------- | ----------------- | -------------------------------------------- |
| Variable | `JF_URL`          | JFrog Platform URL，如 `https://acme.jfrog.io` |
| Secret   | `JF_ACCESS_TOKEN` | 需具备 Deploy / Build Info / Xray Scan 权限       |
| Secret   | `JF_USER`         | Artifactory 用户名（缺少时 publish 可能 401）          |




### Artifactory 仓库配置

流水线通过 `GRADLE_REPO_RESOLVE` / `GRADLE_REPO_DEPLOY` 指向仓库（Gradle/Android 一般为 **Maven** 类型仓库）。请先在 Artifactory 中创建：


| 类型      | 示例名称                         | 说明                                                               |
| ------- | ---------------------------- | ---------------------------------------------------------------- |
| Local   | `guoyz-github-maven-local`   | 存放本流水线部署的制品                                                      |
| Remote  | `guoyz-github-maven-remote`  | 代理中央仓库，URL 为 `https://repo1.maven.org/maven2/`                   |
| Remote  | `guoyz-github-gradle-plugin-remote`  | 代理中央仓库，URL 为 `https://plugins.gradle.org/m2/`                   |
| Virtual | `guoyz-github-maven-virtual` | 聚合上述 local + remote；**Default Deployment Repository** 指向对应 local |


说明：

流水线中使用了 build-scan, 需将相关仓库和 build 加入 **Xray Indexed Resources**，以便 `jf build-scan` 可扫描依赖与制品。

### 可调环境变量


| 变量                       | 默认值                           | 说明       |
| ------------------------ | ----------------------------- | -------- |
| `JFROG_CLI_BUILD_NAME`   | `guoyz-github-gradle-example` | Build 名称 |
| `JFROG_CLI_BUILD_NUMBER` | `${{ github.run_number }}`    | Build 编号 |
| `GRADLE_REPO_RESOLVE`    | `guoyz-github-maven-virtual`  | 解析仓库     |
| `GRADLE_REPO_DEPLOY`     | `guoyz-github-maven-local`    | 部署仓库     |




## 核心 JFrog CLI 步骤


| 步骤            | 命令                                                                               |
| ------------- | -------------------------------------------------------------------------------- |
| 配置 Gradle 仓库  | `jf gradle-config`                                                               |
| 构建并上传         | `jf gradle clean assembleRelease artifactoryPublish --build-name/--build-number` |
| 搜集环境信息        | `jf rt build-collect-env`                                                        |
| 搜集 Git 信息     | `jf rt build-add-git`                                                            |
| 发布 Build Info | `jf rt build-publish`                                                            |
| 扫描 Build      | `jf build-scan`                                                                  |




## 参考链接

- [安装 JFrog CLI](https://docs.jfrog.com/integrations/docs/download-and-install-the-jfrog-cli)
- [JFrog CLI 快速开始](https://docs.jfrog.com/integrations/docs/jfrog-cli-quick-start)
- [JFrog CLI 文档总览](https://docs.jfrog.com/integrations/docs/jfrog-cli)
- [jf gradle 命令说明](https://docs.jfrog.com/artifactory/docs/jf-gradle)

