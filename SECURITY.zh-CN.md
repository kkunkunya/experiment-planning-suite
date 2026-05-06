# 安全策略

安全问题请私下联系仓库 owner。

不要在公开 issue 里包含 secret、客户文件、私有 endpoint、生成日志或未公开研究数据。

## Secret 策略

本仓不应包含：

- API key、token、cookie 或 private key
- 私有项目材料或客户材料
- 带敏感路径或数据的生成日志
- 本地 runtime cache 文件

用户应在本地配置凭据，并把凭据保留在 git 之外。
