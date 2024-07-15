I started off with my nmap scans

ports 21, 22, and 80 seemed to be open

ftp allowed anonymous login so I grabbed the file out of there and also checked to see if I had upload perms on there, which I didn't


I then tried to run a strings on the jpg file that I downloaded from the ftp

I then tried to run a exiftool and binwalk, and couldn't find anything

I then tried to bruteforce the password for the image file, nothing


so I looked at the webpage and started a dirsearch

found /blogs

[12:40:38] 200 -  246B  - /blogs/
 


went into /blogs which nothing was noticable off first glance, so I went into the page source and found a secret directory

 <!--the hint is the another secret directory is S3cr3t-T3xt-->

I went to that secret directory

I looked at the page source of that page and found the key for the image file, in order to extract data out of it, which was :: 3xtr4ctd4t4

<!..Secret Key 3xtr4ctd4t4 >

I then catted the file to find this:
Hello.....  renu 
 
      I tell you something Important.Your Password is too Week So Change Your Password 
Don't Underestimate it....... 

 
I then bruteforced ssh with the username renu to find the password


found a password for user renu... renu:987654321


I then enumerated the machine a little bit to find ways to escalate my privileges. 
I found another user, Lily
I looked at the bash history for renu and found that lily's private key was in renu's home folder
I then logged in as Lily with the private key via ssh
I then I ran a sudo -l and saw that I can run perl as sudo, which would give me root
so I ran sudo perl -e 'exec "/bin/sh";'
and got root.



Hope you enjoyed this writeup! : )
