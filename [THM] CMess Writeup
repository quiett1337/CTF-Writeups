Went through my initial nmap scan and feroxbuster scan, etc to find open ports and directories on the webserver that I can navigate to and possibly find vulnerabilities.

22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0) 
| ssh-hostkey:  
|   2048 d9:b6:52:d3:93:9a:38:50:b4:23:3b:fd:21:0c:05:1f (RSA) 
|   256 21:c3:6e:31:8b:85:22:8a:6d:72:86:8f:ae:64:66:2b (ECDSA) 
|_  256 5b:b9:75:78:05:d7:ec:43:30:96:17:ff:c6:a8:6c:ed (ED25519) 
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu)) 
| http-robots.txt: 3 disallowed entries  
|_/src/ /themes/ /lib/ 
|_http-generator: Gila CMS 
|_http-title: Site doesn't have a title (text/html; charset=UTF-8). 
|_http-server-header: Apache/2.4.18 (Ubuntu) 
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel 


/index                (Status: 200) [Size: 3860]
/about                (Status: 200) [Size: 3359]
/search               (Status: 200) [Size: 3860]
/blog                 (Status: 200) [Size: 3860]
/1                    (Status: 200) [Size: 4090]
/01                   (Status: 200) [Size: 4090]
/01.php               (Status: 200) [Size: 4090]
/1.php                (Status: 200) [Size: 4090]
/login                (Status: 200) [Size: 1583]
/01.txt               (Status: 200) [Size: 4090]
/1.txt                (Status: 200) [Size: 4090]
/category             (Status: 200) [Size: 3871]
/0                    (Status: 200) [Size: 3860]
/feed                 (Status: 200) [Size: 735] 
/themes               (Status: 301) [Size: 324] [--> http://10.10.184.48/themes/?url=themes]
/admin                (Status: 200) [Size: 1583]                                            
/assets               (Status: 301) [Size: 324] [--> http://10.10.184.48/assets/?url=assets]
/tag                  (Status: 200) [Size: 3883]                                            
/author               (Status: 200) [Size: 3599] 



went through every end point and didn't find much that interesting except for /admin, so i'll keep that in mind

I will now try and scan for a sub domain and see if it gives me anything.


I was able to find a dev subdomain

dev
000000019:   200        30 L     104 W      934 Ch      "dev - dev"   


I then navigated to the dev.cmess.thm subdomain and found a user


    
andre@cmess.thm
    Have you guys fixed the bug that was found on live?
            
support@cmess.thm
        Hey Andre, We have managed to fix the misconfigured .htaccess file, we're hoping to patch it in the upcoming patch!
                
support@cmess.thm
        Update! We have had to delay the patch due to unforeseen circumstances
                
andre@cmess.thm
        That's ok, can you guys reset my password if you get a moment, I seem to be unable to get onto the admin panel.
                
support@cmess.thm
        Your password has been reset. Here: KPFTN_f2yxe% 
    


Was able to find andre credentials in dev.cmess.thm 

logged in as andre, who is also an admin


I looked around and tried to upload a reverse shell but was unable to execute it

I then found mysql creds in config.php


$GLOBALS['config'] = array (
  'db' => 
  array (
    'host' => 'localhost',
    'user' => 'root',
    'pass' => 'r0otus3rpassw0rd',
    'name' => 'gila',

I then saw index.php and was able to replace index.php with my own php reverse shell and got a shell

$ whoami
whoami
www-data


I looked around the file system and was able to get andre's password from the /opt folder, in a hidden file



$ ls -la
ls -la
total 12
drwxr-xr-x  2 root root 4096 Feb  6  2020 .
drwxr-xr-x 22 root root 4096 Feb  6  2020 ..
-rwxrwxrwx  1 root root   36 Feb  6  2020 .password.bak

$ cat .password.bak
cat .password.bak
andres backup password
UQfsdCB7aAP6

logged in as andre on the file system and proceeded to find a way to escalate my priviledges to root :)



using the tar2cron privilege escalation vector, I was able to trick root into putting me in the sudoers file to run any command as root with checkpoint.

I was then able to get root access



Thank you for reading! :))

NOTE: This is an older Writeup!




