# API Keys And Local Config

This plugin does not ship API keys, service tokens, cookies, or private endpoints.

Most planning tasks can run from local project text files. If your runtime uses external search, model providers, document conversion tools, or repository APIs, configure those credentials outside this repository.

Recommended pattern:

1. Keep real credentials in your local runtime secret store or a private config file.
2. Use `examples/local-config.example.txt` only as a placeholder template.
3. Never commit `.env`, `.env.*`, generated logs, customer files, or private screenshots.
4. Verify secret presence without printing secret values.

Before publishing derived work, run a secret scan such as:

```bash
rg -n "sk-[A-Za-z0-9_-]{20,}|gh[opsu]_[A-Za-z0-9_]{20,}|AIza[0-9A-Za-z_-]{20,}|BEGIN [A-Z ]*PRIVATE KEY" .
```
