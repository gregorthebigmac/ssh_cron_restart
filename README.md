# ssh_cron_restart

This is a cron job workaround for Debian-based systems where ssh fails on system boot. Obviously, a permanent solution is needed, but I don't know why it's failing yet, so this will have to suffice for now.

## Instructions

So the way this works is as follows:

1. Copy the file restart_sshd.sh somewhere into the root user directory, and remember where you put it.
  A. (I put mine into a special directory dedicated to root scripts. You might want to do the same for better organization, but you do you).
2. Open crontab (as root)
    ```
    # crontab -e
    ```
3.  Copy+paste the following into the end of the crontab file, replacing the path with wherever you stored restart_sshd.sh:
    ```
	@reboot sleep 20 && /root/scripts/restart_sshd.sh
	```
4. Save and exit.

Now, whenever your machine reboots (or just powers on), it will wait 20 seconds after rebooting (which should give it enough time for the networking functions to come online), and *then* it will restart the sshd service. As far as I can tell, the cause of the sshd problem failing on boot is simply because it's trying to fire up the sshd service before networking functions are done, and it has a network connection. By using this cronjob, it makes sure that enough time has passed that if it's going to get a network connection, it should be active by the time the cronjob runs, and sshd should be working shortly after booting up.
