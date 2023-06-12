<pre>
<b>change root password on RHEL9</b>
mount -o remount,rw /
passwd root
touch ./autorelabel (for selinux)
exec /usr/lib/systemd/systemd
</pre>

! [Alt text] (https://github.com/4msahsan/Linux/blob/main/RHEL9/root-password/png/rootpw02.png "msahsan@hotmail.com")


