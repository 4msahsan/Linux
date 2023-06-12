<pre>
<b>change root password on RHEL9</b>
mount -o remount,rw /
passwd root
touch ./autorelabel (for selinux)
exec /usr/lib/systemd/systemd



</pre>

