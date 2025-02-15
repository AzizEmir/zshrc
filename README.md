# zshrc

```zsh
zoxide xclip nala net-tools exa
```

```ZSH
# ---------------------------
# when the (CTRL + A) "^A" key is pressed in the terminal, runs the "z .." command
# ---------------------------
z_up() {
  BUFFER="z .."
  zle .accept-line
}
zle -N z_up
bindkey "^A" z_up


# ---------------------------
# This function will print each directory in the $PATH on a new line, as the tr command replaces the colon (:) separator with a newline (\n).
# ---------------------------
path() {
    echo "$PATH" | tr ':' '\n'
}

# ---------------------------
# Networking
# ---------------------------
alias  myip='curl ifconfig.me'        # Fetch external IP address
alias ports='netstat -tulanp'         # Show active network connections and listening ports

# ---------------------------
# Text Editing
# ---------------------------
alias     v="vim"           # Launch Vim editor
alias zshrc="v ~/.zshrc"    # Open user's zshrc file with vim

# ---------------------------
# Best ifconfig view
# ---------------------------
alias rip="python3 /usr/local/bin/rip.py"

# ────────────────────────────────────────────────────
# 📦 Package Management with Nala  
# ────────────────────────────────────────────────────
alias nalas="nala search"           # 🔍 Search for packages with Nala
alias nalau="nala update"           # 🔄 Update package lists with Nala
alias nalag="nala upgrade"          # ⬆️ Upgrade all packages with Nala
alias nalai="nala install"          # ➕ Install a package using Nala
alias nalap="nala purge"            # ❌ Remove packages completely with Nala

# ────────────────────────────────────────────────────
# 📦 TAR Management: Basic Archive Operations  
# ────────────────────────────────────────────────────
alias tart="tar --list --verbose -f"      # List the contents of the archive
alias tarc="tar --create --verbose -f"    # Create a new archive
alias tarx="tar --extract --verbose -f"   # Extract the archive

# ────────────────────────────────────────────────────
# 🗜️ Gzip Compression with TAR  
# ────────────────────────────────────────────────────
alias  tarz="tar --gzip --create --verbose -f"  # Create a gzip compressed archive
alias tarzx="tar --gzip --extract --verbose -f" # Extract a gzip compressed archive
alias tartz="tar --gzip --list --verbose -f"    # List the contents of a gzip compressed archive

# ────────────────────────────────────────────────────
# 🧳 Bzip2 Compression with TAR  
# ────────────────────────────────────────────────────
alias  tarj="tar --bzip2 --create --verbose -f"  # Create a bzip2 compressed archive
alias tarjx="tar --bzip2 --extract --verbose -f" # Extract a bzip2 compressed archive
alias tartj="tar --bzip2 --list --verbose -f"    # List the contents of a bzip2 compressed archive

# ────────────────────────────────────────────────────
# 🧊 XZ Compression with TAR  
# ────────────────────────────────────────────────────
alias  tarxz="tar --xz --extract --verbose -f" # Extract an XZ compressed archive
alias tarzxz="tar --xz --list --verbose -f"    # List the contents of an XZ compressed archive


# ---------------------------
# Command Line Utilities
# ---------------------------
# ────────────────────────────────────────────────────
# 📜 Show Command History  
# ────────────────────────────────────────────────────
alias h="history"
# ────────────────────────────────────────────────────
# 🔍 Search Command History
# ────────────────────────────────────────────────────
alias hg="history | grep"
# ────────────────────────────────────────────────────
# 🧹 Clear Terminal Screen  
# ────────────────────────────────────────────────────
alias c="clear"

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

# ────────────────────────────────────────────────────
# 📂📜Files:   
# ────────────────────────────────────────────────────
rename_files(){
    # Dosya isimlerini listele (tarih hariç)
    files=$(exa --long --classify --sort=type --no-filesize --no-user --no-permissions | awk '{print $4}')

    # Sayacı başlat
    counter=1

    # Her dosya için yeniden adlandırma işlemi
    for file in ${(f)files}; do
    # Eğer dosya ismi varsa, mv komutuyla yeniden adlandırıyoruz
    if [[ -n "$file" ]]; then
        new_name=$(printf "%02d-%s" $counter "$file")
        mv "$file" "$new_name"
        ((counter++))
    fi
    done
}
# ➜  example-ap git:(main) ✗ rename_files
# yeniden adlandırıldı: 'app/' -> '01-app/'
# yeniden adlandırıldı: 'bootstrap/' -> '02-bootstrap/'
# yeniden adlandırıldı: 'config/' -> '03-config/'
# yeniden adlandırıldı: 'database/' -> '04-database/'
# yeniden adlandırıldı: 'public/' -> '05-public/'
# yeniden adlandırıldı: 'resources/' -> '06-resources/'
# yeniden adlandırıldı: 'routes/' -> '07-routes/'
# yeniden adlandırıldı: 'storage/' -> '08-storage/'
# yeniden adlandırıldı: 'tests/' -> '09-tests/'
# yeniden adlandırıldı: 'vendor/' -> '10-vendor/'
# yeniden adlandırıldı: 'README.md' -> '11-README.md'
# yeniden adlandırıldı: 'composer.json' -> '12-composer.json'
# yeniden adlandırıldı: 'composer.lock' -> '13-composer.lock'
# yeniden adlandırıldı: 'package.json' -> '14-package.json'
# yeniden adlandırıldı: 'phpunit.xml' -> '15-phpunit.xml'
# yeniden adlandırıldı: 'postcss.config.js' -> '16-postcss.config.js'
# yeniden adlandırıldı: 'tailwind.config.js' -> '17-tailwind.config.js'
# yeniden adlandırıldı: 'vite.config.js' -> '18-vite.config.js'

# ---------------------------
# Faster way to navigate your filesystem
# ---------------------------
eval "$(zoxide init zsh)"
```


```python
cat << 'EOF' > /usr/local/bin/rip.py
import re
import subprocess

# ifconfig çıktısını al
ifconfig_output = subprocess.check_output(['ifconfig'], text=True)

# Regex desenleri ve renk kodlarını tutan bir sözlük
patterns_and_colors = {
    # 'prefixlen' değerini tespit et ve magenta (pembe) renkte vurgula
    r'(?<=prefixlen\s)\d+': "\033[38;2;255;0;255m",  # magenta 	#FF00FF rgb(255,0,255)
    
    # 'scopeid' değerini tespit et ve magenta (pembe) renkte vurgula
    r'(?<=scopeid\s0x)[a-fA-F0-9]+': "\033[38;2;255;0;255m",  # magenta 	#FF00FF rgb(255,0,255)
    
    # 'KiB' veya 'MiB' ile biten sayıları tespit et ve magenta (pembe) renkte vurgula
    r'\(\d+(\.\d+)?\s(KiB|MiB|GiB|B)\)': "\033[38;2;255;0;255m",  # magenta 	#FF00FF rgb(255,0,255)
    
    # Ağ yapılandırma ile ilgili anahtar kelimeleri (inet6, ether, prefixlen, vb.) sarı renkte vurgula
    r'\b(inet6|inet|netmask|broadcast|ether|prefixlen|scopeid|RX packets|TX packets)\b': "\033[38;2;255;200;0m",  # yellow #FF8C00 rgb(255,140,0)
    
    # IPv4 adreslerini tespit et ve magenta (pembe) renkte vurgula
    r'(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}': "\033[38;2;255;0;255m",  # magenta #FF00FF rgb(255,0,255)
    
    # Arayüz isimlerini (örneğin 'eth0', 'wlan0') tespit et ve mavi renkte vurgula
    r'^([\w-]+):': "\033[38;2;30;144;255m",  # rgb(30, 144, 255) mavi
    
    # MAC adreslerini tespit et ve yeşil renkte vurgula
    r'(?:[0-9A-Fa-f]{2}[:-]){5}[0-9A-Fa-f]{2}': "\033[38;2;0;255;144m",  # rgb(0, 255, 144) yeşil
    # ipv6
    r'(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])) ':"\033[38;2;255;0;255m", # mediumpurple 	#9370DB 	rgb(147,112,219)
}

# Her bir regex deseni ve renk kodu için işlemleri uygula
highlighted_output = ifconfig_output

for pattern, color_code in patterns_and_colors.items():
    # Eşleşen kısımları renklendirmek için regex'i uygula
    highlighted_output = re.sub(pattern, lambda match: f"{color_code}{match.group(0)}\033[0m", highlighted_output, flags=re.M)

# Renklendirilmiş çıktıyı terminale yazdır
print(highlighted_output)
EOF

sudo chmod +x /usr/local/bin/rip.py
```