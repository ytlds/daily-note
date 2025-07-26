## 1. 常用插件

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
    
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
    
git clone https://github.com/zdharma-continuum/fast-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting
    
git clone --depth 1 -- https://github.com/marlonrichert/zsh-autocomplete.git $ZSH_CUSTOM/plugins/zsh-autocomplete
```

## 2. 常用配置

```
# language
export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# -f 删除免确认
setopt rmstarsilent

# from bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias grep="grep --color=auto"

# git
alias gba='git branch -a'
alias gbd='git branch -D'
alias gck='git checkout'
alias gs='git status'
alias gsub='git submodule update --init --recursive'
```

## 3. shell 函数

```
# rm -> trash, 在 home/user/.Trash 目录下创建一个目录，按照时间命名，将删除的文件和目录移动到该目录下
# 创建一个函数来处理文件删除
trash() {
    local trash_dir="$HOME/.Trash/$(date +%Y-%m-%d)/$(date +%H%M%S%3N)"
    mkdir -p "$trash_dir"
    local args=()
    
    # 过滤掉 rm 的常用参数
    for arg in "$@"; do
        case "$arg" in
            -i|-f|-r|-rf|-fr|--force|--recursive)
                continue
                ;;
            *)
                args+=("$arg")
                ;;
        esac
    done
    
    # 如果过滤后没有任何文件参数，直接返回
    if [ ${#args[@]} -eq 0 ]; then
        return 0
    fi
    
    # 处理剩余的文件参数
    for file in "${args[@]}"; do
        if [ -e "$file" ]; then
            mv "$file" "$trash_dir/"
        else
            echo "警告: $file 不存在"
        fi
    done
}
alias rm='trash'
```