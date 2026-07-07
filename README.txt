
####### USING THE KB COMMANDS:

Use these commands day to day:

kb — jump into the knowledge base directory.

kbls — list every Markdown note.

kbn cisco-switch-baseline — create or open a note by name.

kbf — fuzzy-pick a note by filename with preview.

kbg wireguard — grep note contents for a keyword.

kbv — preview a chosen note in the terminal.

kbs ospf — search note contents interactively and jump to the matching line.

The most useful pair in real work is usually:

kbs <keyword> when you remember a phrase or command.

kbf when you remember the note title but not the exact path.


#### DAILY DRIVING GITHUB
cd ~/kb

# check what changed
git status

# stage everything
git add .

# commit with a useful message
git commit -m "Update WireGuard and drive mounting notes"

# sync to GitHub
git push

######### MAC INSTALLATION TO BUILD KB

brew install fzf ripgrep bat

echo 'source <(fzf --zsh)' >> ~/.zshrc
source ~/.zshrc

Put these functions in ~/.zshrc if you use zsh, or ~/.bashrc if you use bash

export KB_DIR="$HOME/kb"
export EDITOR="${EDITOR:-vim}"

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
