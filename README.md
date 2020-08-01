# log-rotations
This repo elabroates how to apply log rotations to target directory of logs

Why we need log rotations?

Before diving into log rotations I would like to coin following questions

Have you ever face that your production application machine got stuck suddenly with this error "there is no disk space"?
Have you ever face that your mongodb instance disk has been filled due to error floods?
Have you ever experiennced that you are trying to complete command in linux cmd using tab button and it's showing there is no disk space?

So, Here log rotations come in picture to save from error "No Disk Space"

Solution
I am elabratating to keep in mind ubuntu os

logrotate is command in ubuntu, we will use it. We will create two logrotate.conf configuration file and logrotate-state. In logrotate.conf file we will setup to rotate logs based on daily and compress to them.
In logrotate-state let you know last log rotation time and which files has been rotated.


#create logrotate.conf file
```bash
$ touch logrotate.conf
```

put following lines in logrotate.conf

```text
$HOME/path/to/logs-directory/*.log {

	  daily
	  
	  missingok
	  
	  rotate 4
	  
	  compress
	  
	  create
}
```

Create a empty file `logrotate-state` and leave it blank
```bash
$ touch logrotate-state
```

Run following command  
```bash
$/usr/bin/logrotate   logrotate.conf --state logrotate-state
```
If you check logrotate-state you will get info regrading on which files log rotation will be applied.

Now we will put it into cronjob to run this command on every 6 hour so that log rotations will be applied.
```console
$ crontab -e 
0 */6 * * *  /usr/bin/logrotate   $HOME/path/to/logrotate.conf --state $HOME/path/logrotate-state
```
