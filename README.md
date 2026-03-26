# claude-config

Claude Code 配置文件集合，用于跨项目复用 Claude 设置。

## 包含文件

- `.claude/settings.json` - Claude Code 权限和全局设置
- `.claude/settings.local.json` - 本地项目特定权限
- `CLAUDE.md` - 项目代码规范和架构约束

## 使用方法

### 快速安装

```bash
# 克隆仓库
git clone https://github.com/HY-LiYihan/claude-config.git

# 复制到项目
cp -r claude-config/.claude /path/to/your/project/
cp claude-config/CLAUDE.md /path/to/your/project/
```

### 自动同步脚本

在项目根目录创建 `sync-claude-config.sh`：

```bash
#!/bin/bash
REPO_URL="https://github.com/HY-LiYihan/claude-config.git"
TEMP_DIR=$(mktemp -d)
git clone "$REPO_URL" "$TEMP_DIR"
cp -r "$TEMP_DIR/.claude" .
cp "$TEMP_DIR/CLAUDE.md" .
rm -rf "$TEMP_DIR"
echo "Claude config synced!"
```

## 文件说明

### settings.json
全局权限配置，定义 Claude 可以执行的 Bash 命令和文件读取权限。

### settings.local.json
项目特定的本地权限覆盖，用于项目特定的工具调用。

### CLAUDE.md
项目代码规范、架构约束、文档同步规则等。

## 更新

当需要更新配置时，直接修改此仓库中的文件并推送，然后在各项目中重新运行同步脚本。
