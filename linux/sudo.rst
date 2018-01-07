sudo
=====

| ``Defaults   env_reset``
| ``Defaults   mail_badpass``
| ``Defaults   secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/ bin:/sbin:/bin"``
| ``Defaults   logfile=/var/log/sudo.log``

``# Host alias specification``

``# User alias specification``

| ``# Cmnd alias specification``
| ``Cmnd_Alias   BASE = /bin/ls, /bin/cat, /usr/bin/tail, /usr/bin/tailf, /usr/bin/apt-get``
| ``Cmnd_Alias   LIBVIRT = /usr/bin/virt-install, /usr/bin/virsh``
| ``Cmnd_Alias   SALT = /usr/bin/salt, /usr/bin/git, /usr/bin/vi, /usr/bin/vim, /bin/cp, /bin/mkdir``

| ``# User privilege specification``
| ``root   ALL=(ALL:ALL) ALL``
| ``lfzyx   ALL=(root:root) PASSWD:ALL, NOPASSWD:LIBVIRT, NOPASSWD:BASE``

| ``# Allow members of group sudo to execute any command``
| ``# %sudo    ALL=(ALL:ALL) NOPASSWD: ALL,!/bin/su``

| ``vi /etc/rsyslog.conf``
| `` local2.debug    /var/log/sudo.log``

``service rsyslog restart``
