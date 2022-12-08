# Exam_2420
**Part 1 1 point**

How could you update most of the software on your Ubuntu OS?

You don’t have to run the command(s)
Add command(s) to your README.md file in a code block

`sudo apt update`

`sudo apt upgrade`

**Part 2 4 points**

Fix the code with Vim

Copy the code below into a new vim buffer

![image](https://user-images.githubusercontent.com/97474900/206573235-fc5e99e9-2cdc-4c6b-ad7b-ca6b22094aa7.png)

change the code that you just copied so that it matches the image below

![image](https://user-images.githubusercontent.com/97474900/206573271-25b217ef-886b-479b-aca2-c0eb8bc0f5e4.png)

Make sure you get all of the edits.

For each edit list the keybindings used to make changes.
Full marks will be given for a reasonably efficient solution.

Submit a screenshot of the edited file and notes on the keybindings used to make the changes

**Part 3 3 points**

journalctl is used to query journals written by journald. In other words the primary tool for interacting with logs in Linux OSs that use systemd.

Using the man page for journalctl write a journalctl command that does the following:
- print logs for the current boot
- logs should have a priority of warning or more important
- output in a nice pretty json.

As you work through the man page take screenshots of where you found the information
Include a note with each screenshot on how you found the correct options. ie how did you search for it in the man page.

Take a screenshot of your command when you have a working command

![1](https://user-images.githubusercontent.com/97474900/206573326-dea3bd9c-e44f-4ad4-a41f-a48d6215f018.png)
using man page to find -b to print logs for the current boot


![2](https://user-images.githubusercontent.com/97474900/206573677-7aacde70-1d26-4022-b0d1-cf22eb5abee1.png)
using man page to find -p to have logs having a priority of warning or more important


![3](https://user-images.githubusercontent.com/97474900/206573772-8755da66-7da8-4883-b95b-94a3804c7d95.png)
using man page to print in nice json format

![4](https://user-images.githubusercontent.com/97474900/206573829-086c5f0f-0527-4491-9ce1-4009510b7318.png)
final command

**Part 4 5 points**

create a new regular user `adduser` or `useradd` with a home directory. You just need more than one regular user on the system to test your script.

In the midterm you had to use grep to find all of the regular users that exist on the system.
Write a script that finds all of the regular users on a system (for this exercise all of the users with a UID from 1000 to 5000) 
Just like in the midterm find the regular users by searching a file in /etc
That file will contains the users name, UID and GID (these numbers should be the same) as well as home directory and login shell, which is likely bash.

Use grep or awk to find the data

print output that looks like the screenshot below.
The users name, the users UID and the users login shell.

Also print all of the currently logged in users using either the `w` command or the `who` command
Only print their username.

![image](https://user-images.githubusercontent.com/97474900/206573930-3c6ed9ec-2888-49ca-a08f-efb3f5ba3b2b.png)

After you have this completed change your script so that it writes this data to the **motd** file. This is the file you wrote your weather data to in the get_weather script from class.
You want a new version of this data every time the script is run. don’t append to the file.

put your script somewhere logical so that it can be run by a service file.

copy and paste your script into your README inside of a code block

```
#!/bin/bash

# Find regular users with a UID between 1000 and 5000
# and print their name, UID, and login shell
grep -E '^[^:]+:[^:]+:[1-4][0-9]{3}:' /etc/passwd | awk -F: '{print $1 " " $3 " " $7}'

# Print currently logged in users
who | awk '{print $1}'

# Write data to /etc/motd
echo "Regular users:" > /etc/motd
grep -E '^[^:]+:[^:]+:[1-4][0-9]{3}:' /etc/passwd | awk -F: '{print $1 " " $3 " " $7}' >> /etc/motd
echo "Logged in users:" >> /etc/motd
who | awk '{print $1}' >> /etc/motd
```
**Part 5 3 points**

Write a service file that runs your script from part 4

If you couldn’t complete Part 4 just use a script that writes to file for this

```bash
#!/bin/bash

echo "hi Bob" > /home/vagranthi-bob
```

After you have written your service enable and start the service.

Check the status of your service and take a screenshot of the command and the output

add a note to your README on where you put your service file

copy and paste your service file into the README in a code block

```
[Unit]
Description=Find regular users and logged in users

[Service]
Type=simple
ExecStart=/usr/bin/find_user.service

[Install]
WantedBy=multi-user.target

```
- Note I put the servce file in usr/bin

**Part 6 4 points**

Write a timer that runs your service above when you start your system. A **Monotonic timer.**

Your timer should run 1 minute after booting and again everyday while the unit is active.

Start and enable your timer.

Check the status of your timer and take a screenshot of the command and the output

copy and paste your timer file into the README in a code block

hint: `man systemd.timer`

```
[Unit]
Description=Run find_user.service 1 minute after boot and every 24 hours

[Timer]
OnBootSec=1min
OnUnitActiveSec=24h

[Install]
WantedBy=timers.target
```



