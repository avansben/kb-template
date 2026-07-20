# Terminal Knowledge Base (KB)

Quick command reference for the terminal-based documentation setup.

## Commands first

```bash
# Go to the KB root
kb

# List all notes
kbls

# Create or open a note
kbn note-name

# Fuzzy-find a note by filename and open it
kbf

# Search all note contents for a string
kbg wireguard

# Interactive content search and jump to match
kbs ospf

# Preview a note in the terminal
kbv

# Check repo status
cd ~/kb && git status

# Stage all KB changes
cd ~/kb && git add .

# Commit staged KB changes
cd ~/kb && git commit -m "Update KB notes"

# Push commits to GitHub
cd ~/kb && git push
```

## What this KB is

This KB is a local Markdown notes repository designed for terminal-first access, fast copy-paste, and lightweight version control. The workflow uses `fzf` shell integration for fuzzy selection and previews, `ripgrep` for recursive content search, and `bat` for readable terminal previews with syntax highlighting.[1][2][3]

## Core tools

- `fzf` provides shell integration for zsh and supports preview windows plus key bindings such as CTRL-T and CTRL-R.[1]
- `ripgrep` is used as the fast recursive search engine for note contents and works well with `fzf` in interactive search flows.[2][4]
- `bat` is a `cat`-style viewer with syntax highlighting that works well as the preview command inside `fzf`.[3][1]
- Git tracks local history in the repository, and GitHub can be added as an HTTPS remote for backup and sync.[5][6]

## How the KB was created

### 1. Install the required tools

On macOS with Homebrew:

```bash
brew install fzf ripgrep bat
```

Then enable zsh shell integration for `fzf`:

```bash
echo 'source <(fzf --zsh)' >> ~/.zshrc
source ~/.zshrc
```

`fzf` documents zsh shell integration using `source <(fzf --zsh)`, and its preview behavior can be customized through `FZF_CTRL_T_OPTS` and related variables.[1]

### 2. Create the KB folder structure

```bash
mkdir -p ~/kb/{runbooks,linux,networking,security,proxmox,docker,scripts,templates}
```

This structure keeps notes grouped by practical areas while remaining shallow enough for fast fuzzy selection in the terminal.

### 3. Create the note template

```bash
cat > ~/kb/templates/note-template.md <<'EOF2'
# Title

## Purpose
What this process is for.

## Prereqs
Accounts, IPs, tools, access, assumptions.

## Steps
1. 
2. 
3. 

## Commands
```bash
# paste commands here
```

## Verification
```bash
# checks / show commands / curl tests / ping tests
```

## Rollback
```bash
# undo or recovery commands
```

## Notes
Extra context, gotchas, links, ticket refs.
EOF2
```

This uses a shell here-document to write a multi-line Markdown template directly into a file from the terminal.

### 4. Add KB functions to `~/.zshrc`

Add the following to `~/.zshrc`:

```bash
export KB_DIR="$HOME/kb"
export EDITOR="nano"

kb() {
  cd "$KB_DIR" || return
}

kbls() {
  find "$KB_DIR" -type f -name "*.md" | sort
}

kbn() {
  local name="$1"
  if [ -z "$name" ]; then
    echo "Usage: kbn note-name"
    return 1
  fi

  local file="$KB_DIR/${name}.md"

  if [ -e "$file" ]; then
    "$EDITOR" "$file"
    return
  fi

  cp "$KB_DIR/templates/note-template.md" "$file"
  sed -i.bak "s/^# Title/# ${name}/" "$file" && rm -f "${file}.bak"
  "$EDITOR" "$file"
}

kbf() {
  local file
  file=$(find "$KB_DIR" -type f -name "*.md" | \
    fzf --height=80% --layout=reverse --border \
        --preview 'bat --style=plain --color=always {}' \
        --preview-window=right:60%)
  [ -n "$file" ] && "$EDITOR" "$file"
}

kbg() {
  if [ -z "$1" ]; then
    echo "Usage: kbg search-term"
    return 1
  fi

  rg -n --hidden --glob "*.md" "$1" "$KB_DIR"
}

kbv() {
  local file
  file=$(find "$KB_DIR" -type f -name "*.md" | \
    fzf --height=80% --layout=reverse --border \
        --preview 'bat --style=plain --color=always {}' \
        --preview-window=right:60%)
  [ -n "$file" ] && bat --paging=always --style=plain "$file"
}

kbs() {
  local result
  result=$(rg -n --hidden --glob "*.md" --color=always "${1:-.}" "$KB_DIR" | \
    fzf --ansi --delimiter=: \
        --preview 'bat --color=always --highlight-line {2} {1}' \
        --preview-window=right:70%)
  [ -n "$result" ] || return

  local file line
  file=$(echo "$result" | cut -d: -f1)
  line=$(echo "$result" | cut -d: -f2)
  "$EDITOR" "+${line}" "$file"
}
```

Reload the shell after saving:

```bash
source ~/.zshrc
```

This setup matches documented `fzf` shell integration and uses `bat` previews plus `ripgrep` search as recommended patterns for interactive terminal workflows.[1][2][3]

## Daily use

### Create a new note

```bash
kbn cisco-switch-baseline
```

This creates `~/kb/cisco-switch-baseline.md` from the template if it does not already exist, then opens it in the configured editor.

### Find a note by name

```bash
kbf
```

This launches `fzf`, shows a preview with `bat`, and opens the selected file in the editor.[1][3]

### Search note contents

Quick grep-style search:

```bash
kbg wireguard
```

Interactive search with preview and jump-to-line behavior:

```bash
kbs vlan
```

This uses `ripgrep` output as the input list and `fzf` as the interactive selector, which is a standard and effective integration pattern.[2][4]

### Preview a note without editing

```bash
kbv
```

This opens a terminal pager view using `bat` after fuzzy selection, which is useful when the goal is to copy a command without modifying the note.[3]

## Recommended note format

Use one note per process, not one note per giant topic. Good filenames include:

- `wireguard-client-build.md`
- `cisco-switch-baseline.md`
- `nmap-enum-checklist.md`
- `drive-mounting-commands.md`
- `incident-first-10-minutes.md`

A practical note layout is:

- `Purpose` for intent.
- `Prereqs` for access, tools, IPs, or assumptions.
- `Steps` for the workflow.
- `Commands` for copy-paste blocks.
- `Verification` for checks.
- `Rollback` for undo or recovery.
- `Notes` for gotchas and context.

## Git workflow

Git works locally even before a remote is added because the full history is stored inside the repository’s `.git` directory. GitHub setup is only needed when the notes should also be backed up or synced to a remote repository.[7][8]

### Initialize the KB repo

```bash
cd ~/kb
git init
git branch -M main
git add .
git commit -m "Initial KB setup"
```

### Connect GitHub over HTTPS

1. Create an empty private repository on GitHub without adding a README or license.[5][6]
2. Copy the HTTPS remote URL from the green **Code** button.[5][6]
3. Add the remote locally:

```bash
cd ~/kb
git remote add origin https://github.com/YOURUSER/kb.git
```

If the remote already exists and needs to be changed from SSH to HTTPS:

```bash
git remote set-url origin https://github.com/YOURUSER/kb.git
```

### Basic Git commands

```bash
# See what changed
git status

# Stage changes for the next commit
git add .

# Create a local snapshot
git commit -m "Update KB notes"

# Send local commits to GitHub
git push -u origin main
```

`git add` stages changes, `git commit` records the staged snapshot in local history, and `git push` sends local commits to the configured remote repository.[9][10][11]

## Useful tips

- If using zsh, `~/.zshrc` is the shell startup file that loads aliases, functions, and exported variables when a new interactive shell starts.[1]
- If `code` is installed in PATH, `code ~/.zshrc` can be used to edit the shell config in VS Code after enabling the shell command from the Command Palette in VS Code on macOS.[12][13]
- If `nano` is preferred over `vim`, set `export EDITOR="nano"` in `~/.zshrc`, and optionally set Git’s editor with `git config --global core.editor "nano"`.[14][15]
- For process lookups, `ps aux | grep "[c]hrome"` avoids matching the `grep` process itself.[16][17]

## Minimal checklist

```bash
# 1. Install tools
brew install fzf ripgrep bat

# 2. Enable fzf in zsh
echo 'source <(fzf --zsh)' >> ~/.zshrc
source ~/.zshrc

# 3. Create KB folders
mkdir -p ~/kb/{runbooks,linux,networking,security,proxmox,docker,scripts,templates}

# 4. Add note template
# (use the heredoc command shown above)

# 5. Add KB functions to ~/.zshrc
# (use the function block shown above)

# 6. Reload shell
source ~/.zshrc

# 7. Start using it
kbn first-note
kbf
kbg ssh

# 8. Put it under Git
cd ~/kb && git init && git add . && git commit -m "Initial KB setup"
```
