<pre>
<b>change root password on RHEL9</b>
mount -o remount,rw /
passwd root
touch ./autorelabel (for selinux)
exec /usr/lib/systemd/systemd
</pre>

![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw02.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw03.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw04.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw05.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw06.png "msahsan@hotmail.com")
![Alt text](https://github.com/4msahsan/Linux/blob/main/RHEL9/png/rootpw01.png "msahsan@hotmail.com")

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

<pre> 
<h1>Verify /etc/fstab error using findmnt --verify</h1> 


<b>[root@rhel9 root-password]# findmnt --verify</b>
/boot
   [E] unreachable on boot required source: UUID=$$e628c1a6-1984-431f-8300-bbbc631418f7

0 parse errors, 1 error, 0 warnings
[root@rhel9 root-password]#

[root@rhel9 root-password]# findmnt --verify
none
   [E] unreachable on boot required target: No such file or directory
   [W] swa seems unsupported by the current kernel
   [E] swa does not match with on-disk swap

0 parse errors, 2 errors, 1 warning
[root@rhel9 root-password]# cat /etc/fstab

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
/dev/mapper/rhel-swap   none                    swa    defaults        0 0

[root@rhel9 root-password]# vim /etc/fstab
[root@rhel9 root-password]# findmnt --verify
Success, no errors or warnings detected
</pre>


<b> <Top Command> </b>
<pre>
top - 15:38:12 up  4:07,  1 user,  load average: 0.00, 0.00, 0.00
Tasks: 131 total,   1 running, 130 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.2 hi,  0.0 si,  0.0 st
MiB Mem :   9563.1 total,   9032.8 free,    204.4 used,    325.9 buff/cache
MiB Swap:   4044.0 total,   4044.0 free,      0.0 used.   9113.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   4943 root      20   0       0      0      0 I   0.3   0.0   0:00.12 kworker/1:2-ata_sff
   4944 root      20   0   10552   4056   3316 R   0.3   0.0   0:00.08 top
      1 root      20   0  104368  13848   9380 S   0.0   0.1   0:02.00 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
</pre>

[root@rhel9 root-password]# while true; do true; done &
[1] 4946
[root@rhel9 root-password]# while true; do true; done &
[2] 4947
[root@rhel9 root-password]# while true; do true; done &
[3] 4948
[root@rhel9 root-password]# while true; do true; done &
[4] 4949
[root@rhel9 root-password]#
<pre>
top - 15:39:48 up  4:09,  1 user,  load average: 2.03, 0.53, 0.18
Tasks: 135 total,   5 running, 130 sleeping,   0 stopped,   0 zombie
%Cpu(s): 98.5 us,  0.3 sy,  0.0 ni,  0.0 id,  0.0 wa,  1.2 hi,  0.0 si,  0.0 st
MiB Mem :   9563.1 total,   9031.1 free,    206.1 used,    325.9 buff/cache
MiB Swap:   4044.0 total,   4044.0 free,      0.0 used.   9111.7 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   4946 root      20   0    8944   2204      0 R  45.7   0.0   0:21.10 bash
   4949 root      20   0    8944   3520   1316 R  45.7   0.0   0:18.48 bash
   4947 root      20   0    8944   3520   1316 R  45.3   0.0   0:19.58 bash
   4948 root      20   0    8944   2204      0 R  45.3   0.0   0:18.87 bash
   4945 root      20   0       0      0      0 I   0.3   0.0   0:00.02 kworker/1:1-ata_sff
   4950 root      20   0   10552   4284   3544 R   0.3   0.0   0:00.02 top
      1 root      20   0  104368  13848   9380 S   0.0   0.1   0:02.00 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri
      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq
     10 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rc
	 
	 sleeping 0.0 id system taking 100 resources
	 


</pre>

