# VS Code (Ubuntu) — Developer Commands & Tips

## Assumptions
- OS: Ubuntu 22.04+ (works on 20.04+)
- VS Code: official Microsoft build
- Shell: bash or zsh
- User has sudo access

## Limitations
- Some shortcuts differ if using Wayland vs Xorg
- Keybindings can be overridden by extensions
- Snap vs .deb VS Code builds behave differently (Snap has sandbox limits)

---

## Installation & Setup

### Install VS Code (recommended .deb)
- Download from https://code.visualstudio.com
- Or via terminal:
  sudo apt update  
  sudo apt install wget gpg  
  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg  
  sudo install -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/  
  sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'  
  sudo apt update  
  sudo apt install code  

### Verify
- code --version

### Common Failure
- Snap version blocks debugger/docker access  
  Fix: uninstall snap (sudo snap remove code) and install .deb

---

## Launching VS Code

- Open current folder: code .
- Open specific folder: code ~/projects/app
- Open file: code main.ts
- New window: code -n
- Reuse window: code -r .

---

## Essential Keyboard Shortcuts (Linux)

### Navigation
- Command Palette: Ctrl+Shift+P
- Quick File Open: Ctrl+P
- Go to Symbol: Ctrl+Shift+O
- Go to Line: Ctrl+G
- Switch Tabs: Ctrl+Tab

### Editing
- Multi-cursor: Ctrl+Alt+Up/Down
- Select next occurrence: Ctrl+D
- Select all occurrences: Ctrl+Shift+L
- Move line up/down: Alt+Up/Down
- Copy line up/down: Shift+Alt+Up/Down
- Delete line: Ctrl+Shift+K

### Code Actions
- Quick Fix: Ctrl+.
- Rename Symbol: F2
- Format Document: Shift+Alt+F
- Format Selection: Ctrl+K Ctrl+F

---

## Terminal Integration

- Toggle terminal: Ctrl+`
- New terminal: Ctrl+Shift+`
- Split terminal: Ctrl+Shift+5
- Kill terminal: trash icon or exit

### Set default shell
- Ctrl+Shift+P → Terminal: Select Default Profile

---

## Search & Replace

- Find in file: Ctrl+F
- Replace in file: Ctrl+H
- Find in workspace: Ctrl+Shift+F
- Replace in workspace: Ctrl+Shift+H
- Regex search: toggle .*

---

## Git (Built-in)

### Basics
- Open SCM: Ctrl+Shift+G
- Stage file: +
- Commit: message → Ctrl+Enter
- Sync: status bar sync icon

### Common Issues
- Git not detected  
  Fix: sudo apt install git

---

## Debugging

### Start Debugging
- Run/Debug: F5
- Step Over: F10
- Step Into: F11
- Step Out: Shift+F11
- Stop: Shift+F5

### launch.json
- Auto-generate via Run → Add Configuration
- Per-language configs required (Node, Python, Dart, etc.)

---

## Extensions (Must-Have)

- GitLens
- ESLint
- Prettier
- Docker
- Remote - SSH
- Error Lens
- Material Icon Theme

### Install via CLI
- code --install-extension publisher.extension

---

## Workspace & Settings

### User Settings
- Ctrl+Shift+P → Preferences: Open Settings (UI)
- Or JSON: Preferences: Open Settings (JSON)

### Recommended Settings
- editor.formatOnSave: true
- editor.tabSize: 2 or 4
- files.autoSave: afterDelay
- terminal.integrated.fontSize: 12–14

### Workspace Settings
- Stored in .vscode/settings.json
- Overrides user settings per project

---

## Tasks & Automation

### Run Task
- Ctrl+Shift+B

### tasks.json
- Used for build/test scripts
- Integrates with make, npm, gradle, etc.

---

## File & Explorer Tricks

- Reveal file in OS: Right-click → Reveal in File Manager
- Collapse folders: Shift+Click collapse icon
- Drag tabs to split editor

---

## Performance Tips (Ubuntu)

- Disable unused extensions
- Exclude folders:
  files.watcherExclude
  search.exclude
- Avoid Snap build for large repos
- Increase inotify watchers:
  echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf  
  sudo sysctl -p

---

## Common Failures & Fixes

- VS Code slow on large repo  
  Fix: exclude node_modules/.git/build folders
- Terminal not loading env vars  
  Fix: start VS Code from terminal (code .)
- Debugger not attaching  
  Fix: verify launch.json and runtime path

---

## Useful CLI Flags

- code --diff file1 file2
- code --goto file:line:column
- code --list-extensions
- code --uninstall-extension publisher.extension

---

## Verification Checklist

- code --version works
- Ctrl+Shift+P opens command palette
- Integrated terminal opens
- Git detected
- Debugger runs sample project

---

## Notes
- Keep .vscode folder committed only for shared configs
- Prefer workspace settings over user for teams
- Learn Command Palette first — everything is there
