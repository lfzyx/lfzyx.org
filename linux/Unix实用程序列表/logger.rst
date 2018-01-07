用于从shell接口提交日志至syslog日志记录系统

# vi rsyslog.conf
local5.warning  /var/log/shell.log
# logger -p local5.warning "lfzyx.org"