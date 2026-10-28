# pciegen

准备 PCIe RTL 工程目录，调用 ramgen 生成 RAM 封装，再生成控制器封装和文件列表。

> **版本状态说明**：本项目核心实现已回退到 2026 年 10 月前的稳定基线（commit `fe85916`），移除了 10 月份引入的性能基准测试与实验性解析代码，当前版本为 `0.1.4`。

---

## 版本规范与 Git Tag 要求

1. **版本号对应 Git Tag**：
   - **每个版本号必须对应一个 Git tag**（例如版本 `0.1.4` 对应 tag `v0.1.4` 与 `0.1.4`）。
   - 发布新版本时，必须同步递增 `pyproject.toml` 中的 `project.version`，并在提交后创建对应的 Git tag。
   - 禁止在未打 tag 的情况下直接发布 wheel 包。

2. **打 Tag 命令示例**：
   ```powershell
   git tag v0.1.4
   # 推送标签到远端：
   # git push origin v0.1.4
   ```

---

## 源码运行

将 `pciegen`、`ramgen`、`vutils` 三个独立仓库置于同一父目录下。
在 `pciegen` 仓库根目录运行，Python 版本由 `.python-version` 选择为 3.12：

```powershell
uv run pciegen --help
uv run pciegen --rule examples/rule.csv
```

- `--rule`：指定包装规则 CSV 文件路径（默认优先使用环境变量 `PCIEGEN` 目录下的 `rule.csv`，其次使用源码目录或安装包提供的 `examples/rule.csv`）。
- 亦可直接通过 Python 运行：
  ```powershell
  python pciegen.py --rule examples/rule.csv
  ```

---

## 构建与打包 (whl)

本项目支持通过 `uv` 或标准 build 工具构建 wheel 包：

```powershell
# 清理旧构建产物并构建
Remove-Item -Recurse -Force dist, build -ErrorAction SilentlyContinue
uv build
```

构建成功后，在 `dist/` 目录下将生成：
- `dist/pciegen-0.1.4-py3-none-any.whl`
- `dist/pciegen-0.1.4.tar.gz`

安装生成的 wheel 包：
```powershell
pip install dist/pciegen-0.1.4-py3-none-any.whl
```
安装后即可在系统命令行中直接调用 `pciegen` 命令。
