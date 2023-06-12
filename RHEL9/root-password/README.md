<pre>
<b>change root password on RHEL9</b>
mount -o remount,rw /
passwd root
touch ./autorelabel (for selinux)
exec /usr/lib/systemd/systemd
</pre>

![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw02.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw03.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw04.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw05.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw06.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw01.png "msahsan@hotmail.com")

<pre>
<h1><b> Debug Shell for troubleshooting</h1></b>



</pre>
<pre>
<b>DebugShell gain root access without password</b>

enable debug shell

[root@rhel9 root-password]# systemctl enable --now debug-shell.service
Created symlink /etc/systemd/system/sysinit.target.wants/debug-shell.service → /usr/lib/systemd/system/debug-shell.service.
[root@rhel9 root-password]#

<b> Now doing some changes on fstab so the system having issues during boot.</b>

#
# /etc/fstab
# Created by anaconda on Tue Aug  9 06:32:22 2022
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/rhel-root   /                       xfs     defaults        0 0
UUID=e628c1a6-1984-431f-8300-bbbc631418f7 /boot                   xfs     defaults        0 0
/dev/mapper/rhel-home   /home                   xfs     defaults        0 0
/dev/mapper/rhel-swap   none                    swap    defaults        0 0
~
/dev/mapper/rhel-root   /                       xfs     defaults        0 0
UUID=$$$$e628c1a6-1984-431f-8300-bbbc631418f7 /boot                   xfs     defaults        0 0
/dev/mapper/rhel-home   /home                   xfs     defaults        0 0
/dev/mapper/rhel-swap   none                    swap    defaults        0 0
~
after some changes on fstab reboot the server you saw system is stuck now use Alt+9 key for debug shell update the fstab and reboot

Once system boot without any issue disable boot shell

[root@rhel9 ~]# systemctl disable --now debug-shell.service
Removed /etc/systemd/system/sysinit.target.wants/debug-shell.service.
[root@rhel9 ~]#

</pre>


