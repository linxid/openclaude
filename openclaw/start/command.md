## OpenClaw详细安装流程：
```
curl -fsSL https://openclaw.ai/install.sh | bash
╭────────────────────────────────────────────────────────────────────────────────╮
│                                                                                │
│  🦞 OpenClaw Installer                                                         │
│  I can grep it, git blame it, and gently roast it—pick your coping mechanism.  │
│  modern installer mode                                                         │
│                                                                                │
╰────────────────────────────────────────────────────────────────────────────────╯

✓ gum bootstrapped (temp, verified, v0.17.0)
✓ Detected: macos
            
Install plan
            
OS                  macos
Install method      npm
Requested version   latest
INFO Existing OpenClaw installation detected, upgrading
                           
[1/3] Preparing environment
                           
✓ Homebrew already installed
✓ Node.js v23.11.0 found
                         
[2/3] Installing OpenClaw
                         
✓ Git already installed
INFO Installing OpenClaw v2026.2.19-2
✓ OpenClaw npm package installed
✓ OpenClaw installed
                      
[3/3] Finalizing setup
                      
INFO Running doctor to migrate settings
✓ Doctor complete

🦞 OpenClaw installed successfully (2026.2.19-2)!
Upgraded! Peter fixed stuff. Blame him if it breaks.

INFO Upgrade complete
INFO Running openclaw doctor

🦞 OpenClaw 2026.2.19-2 (45d9b20) — If you're lost, run doctor; if you're brave, run prod; if you're wise, run tests.

▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
██░▄▄▄░██░▄▄░██░▄▄▄██░▀██░██░▄▄▀██░████░▄▄▀██░███░██
██░███░██░▀▀░██░▄▄▄██░█░█░██░█████░████░▀▀░██░█░█░██
██░▀▀▀░██░█████░▀▀▀██░██▄░██░▀▀▄██░▀▀░█░██░██▄▀▄▀▄██
▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
                  🦞 OPENCLAW 🦞                    
 
┌  OpenClaw doctor
│
◇  Gateway ──────────────────────────────────────────────────────────╮
│                                                                    │
│  gateway.mode is unset; gateway start will be blocked.             │
│  Fix: run openclaw configure and set Gateway mode (local/remote).  │
│  Or set directly: openclaw config set gateway.mode local           │
│  Missing config: run openclaw setup first.                         │
│                                                                    │
├────────────────────────────────────────────────────────────────────╯
│
◇  Gateway auth ──────────────────────────────────────────────────────────────────────╮
│                                                                                     │
│  Gateway auth is off or missing a token. Token auth is now the recommended default  │
│  (including loopback).                                                              │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────╯
│
◇  Generate and configure a gateway token now?
│  Yes
│
◇  Gateway auth ──────────────╮
│                             │
│  Gateway token configured.  │
│                             │
├─────────────────────────────╯
│
◇  Create Session store dir at ~/.openclaw/agents/main/sessions?
│  Yes
│
◇  Create OAuth dir at ~/.openclaw/credentials?
│  Yes
│
◇  State integrity ───────────────────────────────────────────────────────────╮
│                                                                             │
│  - CRITICAL: Session store dir missing (~/.openclaw/agents/main/sessions).  │
│  - CRITICAL: OAuth dir missing (~/.openclaw/credentials).                   │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────╯
│
◇  Doctor changes ────────────────────────────────────────────────╮
│                                                                 │
│  - Created Session store dir: ~/.openclaw/agents/main/sessions  │
│  - Created OAuth dir: ~/.openclaw/credentials                   │
│                                                                 │
├─────────────────────────────────────────────────────────────────╯
│
◇  Security ─────────────────────────────────╮
│                                            │
│  - No channel security warnings detected.  │
│  - Run: openclaw security audit --deep     │
│                                            │
├────────────────────────────────────────────╯
│
◇  Skills status ────────────╮
│                            │
│  Eligible: 32              │
│  Missing requirements: 18  │
│  Blocked by allowlist: 0   │
│                            │
├────────────────────────────╯
│
◇  Plugins ──────╮
│                │
│  Loaded: 4     │
│  Disabled: 32  │
│  Errors: 0     │
│                │
├────────────────╯
│
◇  Memory search ───────────────────────────────────────────────────────────────────────────╮
│                                                                                           │
│  Memory search is enabled but no embedding provider is configured.                        │
│  Semantic recall will not work without an embedding provider.                             │
│                                                                                           │
│  Fix (pick one):                                                                          │
│  - Set OPENAI_API_KEY or GEMINI_API_KEY in your environment                               │
│  - Add credentials: openclaw auth add --provider openai                                   │
│  - For local embeddings: configure agents.defaults.memorySearch.provider and local model  │
│    path                                                                                   │
│  - To disable: openclaw config set agents.defaults.memorySearch.enabled false             │
│                                                                                           │
│  Verify: openclaw memory status --deep                                                    │
│                                                                                           │
├───────────────────────────────────────────────────────────────────────────────────────────╯
│
◇  
│
◇  Gateway ──────────────╮
│                        │
│  Gateway not running.  │
│                        │
├────────────────────────╯
│
◇  Gateway connection ───────────────────────────╮
│                                                │
│  Gateway target: ws://127.0.0.1:18789          │
│  Source: local loopback                        │
│  Config: /Users/linxi/.openclaw/openclaw.json  │
│  Bind: loopback                                │
│                                                │
├────────────────────────────────────────────────╯
│
◇  Gateway ────────────────────────╮
│                                  │
│  Gateway service not installed.  │
│                                  │
├──────────────────────────────────╯
│
◇  Install gateway service now?
│  Yes
│
◇  Gateway service runtime
│  Node (recommended)

Installed LaunchAgent: /Users/linxi/Library/LaunchAgents/ai.openclaw.gateway.plist
Logs: /Users/linxi/.openclaw/logs/gateway.log
Updated ~/.openclaw/openclaw.json
│
└  Doctor complete.

INFO Updating plugins
No npm-installed plugins to update.
INFO Gateway daemon detected; restarting
✓ Gateway restarted

🦞 OpenClaw 2026.2.19-2 (45d9b20) — WhatsApp automation without the "please accept our new privacy policy".

Dashboard URL: http://127.0.0.1:18789/#token=2ce604795458c2b93ceed75562ae150036641645a44afb8d
Copied to clipboard.
Opened in your browser. Keep that tab to control OpenClaw.
╭─────────────────────────────────────────╮
│ Need help?                              │
│ FAQ: https://docs.openclaw.ai/start/faq │
╰─────────────────────────────────────────╯
```

## OpenClaw 常用命令：

```
# 1. 安装 openclaw：
curl -fsSL https://openclaw.ai/install.sh | bash

# 2. 查看版本（是否安装成功）
openclaw --version

# 3. 运行初始化向导，很多初始配置的修改也可以通过这行命令
openclaw onboard --install-daemon

# 4. 启动网关
# 前台启动网关
openclaw gateway
# 
# 5. 关闭网关
openclaw gateway stop

重启网关服务：
openclaw gateway restart

查看状态（是否有重复进程）：
openclaw gateway status
lsof -nP -iTCP:18789 -sTCP:LISTEN

怀疑有“僵尸/重复”进程，先彻底清场再启动：
openclaw gateway stop
pkill -f "openclaw-gateway|dist/index.js gateway" || true
openclaw gateway start

# 6. 打开终端页面
openclaw tui

# 7. 查看 gateway 状态
openclaw gateway status

# 8. 查看所有支持的模型
openclaw models list --all


```

## Bug 解决：

### Bug1： gateway connect failed: Error: pairing required

```
Manual Fix

Edit ~/.openclaw/devices/paired.json — find the openclaw-probe device and update both scopes arrays (top-level and inside tokens.operator):
"scopes": ["operator.read", "operator.admin", "operator.approvals", "operator.pairing"]
Edit ~/.openclaw/identity/device-auth.json — same change to tokens.operator.scopes.

Restart the gateway:

openclaw gateway restart
```

### Bug2：disconnected (1008): unauthorized: gateway token missing (open the dashboard URL and paste the token in Control UI settings)

This gateway requires auth. Add a token or password, then click Connect.openclaw dashboard --no-open → tokenized URL

openclaw doctor --generate-gateway-token → set token

Docs: Control UI auth

Token 不匹配问题：
```
在终端中运行以下命令来强制生成一个新的网关 Token：

Bash
openclaw doctor --generate-gateway-token
(注：如果你想查找现有的 Token，也可以查看本地配置文件：grep -A1 '"token"' ~/.openclaw/openclaw.json)

将终端输出的那串 Token 字符复制下来。

回到你目前报错的 Control UI 网页界面。

找到网关访问配置区域（通常在左侧菜单的 Control -> Overview -> Gateway Access Panel 中）。

将复制好的 Token 粘贴到输入框中，然后点击 Connect（连接）。
```
