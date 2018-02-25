alias
========

设置指令的别名
alias ll='ls -lah --color' ls='ls -a --color' df='df -hT' free='free -h' vmstat='vmstat -S M' geoip='geoiplookup' sudo='sudo ' ss='ss -4'
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
永久生效:vi /etc/bash.bashrc，把上面的命令加到后面