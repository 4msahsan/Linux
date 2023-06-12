<pre>
change root password on RHEL9
mount -o remount,rw /
passwd root
touch ./autorelabel (for selinux)
exec /usr/lib/systemd/systemd



</pre>

