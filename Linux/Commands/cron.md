
## sudo apt-get install cron
</br>

## To debug cron:
```
grep CRON /var/log/syslog
```


# Usage 1

Start cron only when power on. If you process needs to run with ```sudo```, you need to edit ```cron``` by ```sudo crontab -e``` otherwise ```crontab -e```. Dont forget to add newline at the end of the file.

## Example:
```
debian@beaglebone:/home$ sudo cat crontab -e
...
...
@reboot sudo /bin/temp.sh

```
```
debian@beaglebone:/home$ cat /bin/temp.sh 
sudo python3 /home/gathercollector.py &
sudo python3 /home/gatherlog.py &
#sudo python3 /home/temp.py &
```


