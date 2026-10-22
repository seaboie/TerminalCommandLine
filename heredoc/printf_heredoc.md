# Printf : Here Doc


```bash
my_function() {
    if [[ "$1" == "--help" || "$1" == "-h" || -z "$1" ]]; then

        printf "%b" "$(cat << "EOF"

        \e[1;33mKritbovorn Taweeyossak\e[0m     # พิมพ์ สีตัวหนังสืออกมา

EOF
        
)"
        return 0
    fi

    printf "%b\n" "\e[1;33mสำเร็จ\e[0m"           # พิมพ์ สีตัวหนังสืออกมา
    
}
```