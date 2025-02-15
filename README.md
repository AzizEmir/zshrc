# zshrc

```zsh
zoxide xclip nala net-tools exa
```


```zsh
# ---------------------------
# when the (CTRL + A) "^A" key is pressed in the terminal, runs the "z .." command
# ---------------------------
z_up() {
  BUFFER="z .."
  zle .accept-line
}
zle -N z_up
bindkey "^A" z_up
```

```zsh
# ---------------------------
# This function will print each directory in the $PATH on a new line, as the tr command replaces the colon (:) separator with a newline (\n).
# ---------------------------
path() {
    echo "$PATH" | tr ':' '\n'
}
# ---------------------------
# The catclip() function in your example takes a file as an argument and copies its contents to the clipboard using xclip
# ---------------------------
catclip() {
    cat "$1" | xclip -selection clipboard
}
```

```zsh
# ---------------------------
# Networking
# ---------------------------
alias  myip='curl ifconfig.me'        # Fetch external IP address
alias ports='netstat -tulanp'         # Show active network connections and listening ports

# Best ifconfig view
rip() {
    ifconfig | awk 'BEGIN {
        # Renk kodları
        BRIGHT_YELLOW="\033[93m"    # Parlak altın sarısı
        BRIGHT_MAGENTA="\033[95m"   # Parlak mor
        BRIGHT_BLUE="\033[94m"      # Parlak mavi
        BRIGHT_GREEN="\033[92m"     # Parlak yeşil
        RESET="\033[0m"
    }
    {
        # Arayüz isimlerini parlak mavi renkle
        if (match($0, /^[a-zA-Z0-9:_]+:/)) {
            interface_name = substr($0, 1, RLENGTH)
            rest_of_line = substr($0, RLENGTH + 1)
            print BRIGHT_BLUE interface_name RESET rest_of_line
        } else {
            # RX packets ve TX packets başlıklarını parlak yeşil ile renklendir
            gsub(/RX packets/, BRIGHT_GREEN "&" RESET)
            gsub(/TX packets/, BRIGHT_GREEN "&" RESET)

            # Boyut bilgilerini parlak yeşil ile renklendir
            gsub(/\([0-9.]+ [KMGTPE]iB\)/, BRIGHT_GREEN "&" RESET)

            # "ether" ve MAC adresini renklendir
            gsub(/ether /, BRIGHT_YELLOW "&" RESET)
            gsub(/[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}/, BRIGHT_MAGENTA "&" RESET)

            # "inet", "netmask", ve "broadcast" kelimelerini parlak altın sarı ile renklendir
            gsub(/inet |netmask |broadcast /, BRIGHT_YELLOW "&" RESET)

            # IP adreslerini parlak mor ile renklendir
            gsub(/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/, BRIGHT_MAGENTA "&" RESET)

            print
        }
    }'
}
```

![colored ifconfig](https://github.com/user-attachments/assets/2177696f-5ca7-432b-a72a-9e36e32d2e98)

```zsh
# ---------------------------
# Privileged Command Shortcut
# ---------------------------
alias _="sudo"                         # Quick sudo shortcut
```

```zsh
# ---------------------------
# Text Editing
# ---------------------------
alias     v="vim"                        # Launch Vim editor
alias    sv="_ vim"                      # Alternate Vim alias
alias zshrc="v ~/.zshrc"                 # Open user's zshrc file with vim
```

```zsh
# ---------------------------
# Package Management with Nala
# ---------------------------
alias nalas="nala search"              # Search for packages with Nala
alias nalau="_ nala update"            # Update package lists with Nala
alias nalag="_ nala upgrade"           # Upgrade all packages with Nala
alias nalai="_ nala install"           # Install a package using Nala
alias nalap="_ nala purge"             # Remove packages completely with Nala
```

```zsh
# ---------------------------
# TAR Management
# ---------------------------
alias  tart='tar --list    --verbose -f'  # List the contents of the archive
alias  tarc='tar --create  --verbose -f'  # Create a new archive
alias  tarx='tar --extract --verbose -f'  # Extract the archive

alias  tarz='tar --gzip --create  --verbose -f' # Create a gzip compressed archive
alias tarzx='tar --gzip --extract --verbose -f' # Extract a gzip compressed archive
alias tartz='tar --gzip --list --verbose -f' # List the contents of a gzip compressed archive

# Aliases by Archive Type
alias  tarj='tar --bzip2 --create  --verbose -f'  # Create a bzip2 compressed archive
alias tarjx='tar --bzip2 --extract --verbose -f'  # Extract a bzip2 compressed archive
alias tartj='tar --bzip2 --list    --verbose -f'  # List the contents of a bzip2 compressed archive

# Aliases by Compression Format
alias  tarxz='tar --xz --extract --verbose -f'  # Extract an XZ compressed archive
alias tarzxz='tar --xz --list    --verbose -f'  # List the contents of an XZ compressed archive
```

```zsh
# ---------------------------
# Weather Utilities
# ---------------------------
alias    wttr='curl wttr.in'           # Fetch weather information
alias weather='curl wttr.in'           # Alternate alias for weather
```

```zsh
# ---------------------------
# Command Line Utilities
# ---------------------------
alias  h='history'                     # Show command history
alias hg='history | grep $1'           # Search command history with a keyword
alias  c='clear'                       # Clear the terminal screen
```

```zsh
# ---------------------------
# File Operations
# ---------------------------
# ────────────────────────────────────────────────────
# 📁 Safe Copy: Interactive & Verbose  
# ────────────────────────────────────────────────────
alias cp="cp --interactive --verbose"
# ────────────────────────────────────────────────────
# 🚚 Safe Move: Interactive & Verbose  
# ────────────────────────────────────────────────────
alias mv="mv --interactive --verbose"
# ────────────────────────────────────────────────────
# 🗑️  Safe Remove: Protect root & Verbose  
# ────────────────────────────────────────────────────
alias rm="rm --interactive --verbose --preserve-root"
# ────────────────────────────────────────────────────
# 📂 Create Directory: Parent directories & Verbose  
# ────────────────────────────────────────────────────
alias mdir="mkdir --parents --verbose"
# ────────────────────────────────────────────────────
# 📂 Create Directory & Move Into It  
# ────────────────────────────────────────────────────
mdircd() {
    mkdir -pv "$1" && z "$1"
}
# ────────────────────────────────────────────────────
# 📂 Enhanced ls: List directories first with icons  
# ────────────────────────────────────────────────────
alias els="exa --group --sort=type --icons"
# ────────────────────────────────────────────────────
# 📜 Extended ls: Show detailed list without size/user  
# ────────────────────────────────────────────────────
alias ell="exa --long --header --classify --no-filesize --no-user --no-permissions"
# ────────────────────────────────────────────────────
# 🎨 Detailed View: Grouped, sorted, and with icons  
# ────────────────────────────────────────────────────
alias elld="exa --long --header --classify --sort=type --group --icons"
# ────────────────────────────────────────────────────
# 🔍 Extended ls (Sorted): Organized, no size/user  
# ────────────────────────────────────────────────────
alias ellt="exa --long --header --classify --sort=type --no-filesize --no-user --no-permissions"
# ────────────────────────────────────────────────────
# 📊 Disk Usage: Show human-readable sizes, sorted  
# ────────────────────────────────────────────────────
alias lu="du --summarize --human-readable * | sort --human-numeric-sort"
```

```zsh
reorder_files_with_regex_pattern() {
    file_list=()
    while IFS= read -r file; do
        file_list+=("$file")
    done < <(ls -1 | sort)
    total_count=${#file_list[@]}
    total_count_digits=${#total_count}
    counter=1
    for file in $file_list; do
        printf "[%0${total_count_digits}d]-%s\n" $counter "$file"
        ((counter++))
    done
}
```

Example:
```plaintext
# > reorder_files_with_regex_pattern 
# [01]-Belgeler
# [02]-Genel
# [03]-İndirilenler
# [04]-Masaüstü
# [05]-Müzik
# [06]-Resimler
# [07]-Şablonlar
# [08]-Videolar
# [09]-VirtualBox_VMs
# [10]-example.sh
```

```zsh
reorder_and_rename_files_with_regex_pattern() {
    file_list=()
    while IFS= read -r file; do
        file_list+=("$file")
    done < <(ls -1 | sort)
    total_count=${#file_list[@]}
    total_count_digits=${#total_count}
    counter=1
    for file in $file_list; do
        new_name=$(printf "[%0${total_count_digits}d]-%s" $counter "$file")
        mv -i "$file" "$new_name"
        ((counter++))
    done
}
```

Example:
```plaintext
# > reorder_and_rename_files_with_regex_pattern
# yeniden adlandırıldı: 'Belgeler' -> '[01]-Belgeler'
# yeniden adlandırıldı: 'Genel' -> '[02]-Genel'
# yeniden adlandırıldı: 'İndirilenler' -> '[03]-İndirilenler'
# yeniden adlandırıldı: 'Masaüstü' -> '[04]-Masaüstü'
# yeniden adlandırıldı: 'Müzik' -> '[05]-Müzik'
# yeniden adlandırıldı: 'Resimler' -> '[06]-Resimler'
# yeniden adlandırıldı: 'Şablonlar' -> '[07]-Şablonlar'
# yeniden adlandırıldı: 'Videolar' -> '[08]-Videolar'
# yeniden adlandırıldı: 'VirtualBox_VMs' -> '[09]-VirtualBox_VMs'
# yeniden adlandırıldı: 'example.sh' -> '[10]-example.sh'
```

```zsh
rename_files_with_pattern_removed() {
    for file in *; do
        # Eğer dosya adı başında [number]- varsa
        if [[ "$file" =~ ^\[[0-9]+\]- ]]; then
            original_name=$(echo "$file" | sed 's/^\[[0-9]\+\]-//')
            mv --interactive "$file" "$original_name"
        fi
    done
}
```

Example:
```plaintext
# > rename_files_with_pattern_removed
# yeniden adlandırıldı: '[01]-Belgeler' -> 'Belgeler'
# yeniden adlandırıldı: '[02]-Genel' -> 'Genel'
# yeniden adlandırıldı: '[03]-İndirilenler' -> 'İndirilenler'
# yeniden adlandırıldı: '[04]-Masaüstü' -> 'Masaüstü'
# yeniden adlandırıldı: '[05]-Müzik' -> 'Müzik'
# yeniden adlandırıldı: '[06]-Resimler' -> 'Resimler'
# yeniden adlandırıldı: '[07]-Şablonlar' -> 'Şablonlar'
# yeniden adlandırıldı: '[08]-Videolar' -> 'Videolar'
# yeniden adlandırıldı: '[09]-VirtualBox_VMs' -> 'VirtualBox_VMs'
# yeniden adlandırıldı: '[10]-example.sh' -> 'example.sh'
```

```zsh
# ---------------------------
# Codium Profiles
# ---------------------------
alias       codium_shell="codium -n --profile Shell_Scripting" # Launch with Shell_Scripting Profile
alias          codium_md="codium -n --profile Markdown"        # Launch with Markdown Profile
alias codium_web_artisan="codium -n --profile Laravel"         # Launch with Laravel Profile
```
