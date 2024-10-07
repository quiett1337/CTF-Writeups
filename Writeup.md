Hi! This is my writeup to Tryhackme's Cheese! Here, I will take you through my thought process with solving this machine!

I started off with an nmap scan and there were lots of  ports open for me. 
![[1 1.png]]

It seems that there are a lot of false positives.. but I did realize that port 22, 80, and 8080 were running for sure!

Let's go to port 80 first!
![[2 1.png]]
Decided to poke around to see what I can find!
I found a login page, interesting.
![[3 1.png]]
Now let's try some stock credentials.
Nothing came from stock credentials
I decided to try SQLi, nothing
okay, Let's do some content discovery!
I found a couple of html files.
![[4.png]]
I decided to go through all of the html files and found that messages.html led to a different endpoint.
![[5.png]]

![[5_1.png]]

This looks like a lfi vuln via PHP Wrapper
It seems I was able to get an LFI, great!
Now having dealt with php filters before, I do know I could get an RCE through php filters
Knowing that, I was able to use php filter chaining to generate a payload to gain a shell.

![[6 2.png]]

![[7 2.png]]


The payload was too long to fit into one screenshot


Now, Time for Privilege Escalation!

Let's do some enumeration!

I looked around and was able to find creds in the login.php file

![[8__1.png]]


I tried switching users to comte using those credentials, unfortunately a no go

I then decided to log into mysql with those credentials, success!

I then dumped user creds and found a hashed password. It looks like md5.
Great!

![[9__1.png]]


Unfortunately, I was unable to crack the hash. Looks like there may be another way of escalating my privileges. Let’s do some more enumeration

I decided to run linpeas next while I do some manual enumeration

I ran a find command to find writable files

I found that /etc/systemd/system/exploit.timer and the authorized_keys in comte's home directory writeable, interesting

Let's try to put our own public key in the authorized_keys and see if we can log in.

![[10.png]]

Great! Now we're comte!

let's what we can run as sudo with the user comte!

![[11.png]]

Looks like we can run 4 systemctl commands as root

Let's look at the exploit.timer and the exploit.service files in the /etc/systemd/system directory


![[12.png]]

![[13.png]]

it looks like the service file copies the xxd command to the /opt directory and give it suid permissions
Maybe we can use gtfobins to exploit those permissions

At this point, I reloaded the daemon and restarted all of the services so that it will copy the xxd file to the /opt directory

xxd was then copied to the /opt/ directory with suid permissions
![[14.png]]

At this point, I used gtfobins to exploit xxd to read the root flag


![[15.png]]

and BAM, Cheese CTF has been pwned!

Hope you guys enjoyed this writeup. There will be more coming soon!