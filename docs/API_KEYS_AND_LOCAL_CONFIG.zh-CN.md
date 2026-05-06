# API Key 与本地配置

本 plugin 不携带 API key、service token、cookie 或私有 endpoint。

多数实验规划任务只需要读取本地项目文本。如果你的 runtime 会调用外部搜索、模型供应商、文档转换工具或代码仓库 API，请在本仓之外配置凭据。

推荐方式：

1. 真实凭据放在本地 runtime secret store 或私有配置文件里。
2. `examples/local-config.example.txt` 只作为占位模板。
3. 不要提交 `.env`、`.env.*`、生成日志、客户文件或私有截图。
4. 验证凭据是否存在时，不要打印凭据值。

发布衍生作品前，运行类似扫描：

```bash
rg -n "sk-[A-Za-z0-9_-]{20,}|gh[opsu]_[A-Za-z0-9_]{20,}|AIza[0-9A-Za-z_-]{20,}|BEGIN [A-Z ]*PRIVATE KEY" .
```
