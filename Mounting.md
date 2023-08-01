#### permanently mounting pendrive with jetson nano
1. Open the /etc/fstab file in a text editor.
2. Add the following line to the file:
```
/dev/sdX /data/pendrive ext4 defaults 0 0
```
/sev/sdX you can find using lsblk command.

3. Save the /etc/fstab file.
4. Restart your computer.

