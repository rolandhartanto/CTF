# CTFLearn Write-ups

Topics:
- [Web Exploitation](#web-exploitation)
- [Miscellaneous](#miscellaneous)
- [Cryptography](#cryptography)
- [Forensics](#forensics)
- [Programming](#programming)
- [Binary Exploitation](#binary-exploitation)

## Web Exploitation
### > Basic Injection - intelagent - 2pts
See if you can leak the whole database. https://web.ctflearn.com/web4/

**Solution:**  
You can input any string to the input field. In this case, you can try SQL Injection to leak the whole database. The flag is in the output.

```
' OR '1' = '1
```
<br>

### > Don't Bump Your Head(er) - intelagent - 4pts
Try to bypass my security measure on this site! https://ctflearn.com/header.php

**Solution:**  
When you visit the site, you can find the clue in the HTML code.
```
<html>
  <head></head>
  <body>
    Sorry, it seems as if your user agent is not correct, in order to access this website. The one you supplied is: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
    
    <!-- Sup3rS3cr3tAg3nt  -->
  </body>
</html>
```

From the clue above, we have to change the user agent to `Sup3rS3cr3tAg3nt` before we send the request to the site.

Using postman, change the user agent to `Sup3rS3cr3tAg3nt` and send GET request to https://ctflearn.com/header.php. You will get this response. 

```
Sorry, it seems as if you did not just come from the site, "awesomesauce.com".
```

You have to change the Referer in the header to awesomesauce.com then you send GET request again to https://ctflearn.com/header.php. The response contains the flag in it.

**Summary:**  
Send request to https://ctflearn.com/header.php with this header:
```
User-Agent: Sup3rS3cr3tAg3nt
Referer: awesomesauce.com
```
<br>

### > POST Practice - intelagent - 6pts
These website requires authentication, via POST. However, it seems as if someone has defaced our site. Maybe these is still some way to authenticate? https://ctflearn.com/post.php

**Solution:**  

## Miscellaneous


## Cryptography


## Forensics
### > Binwalk - alexkato29 - 2pts
https://mega.nz/#!4UEnAZKT!-deNdQJxsQS8bTSMxeUOtpEclCI-zpK7tbJiKV0tXYY

**Solution:**  
Files can be opened by text editor. After opening the file with text editor, you can see the content of the file.
```
‰PNG
SUB
NULNULNUL
IHDR ... the content of the file
...
```
IHDR is usually the format for PNG files. Search for "IHDR" in the file and you will find another "IHDR" string somewhere in the file. It means that that file contains more than one picture (It is possible to put another picture/file after the end of the host file).
```
... first picture
...
‰PNG
SUB
NULNULNUL
IHDR ... the content of the second file
...
```
To show the second picture, you have to delete the first picture. After deleting the first picture, you can see the flag by opening the file with image viewer.

**Summary:**  
1. Delete the first picture to show the second picture
2. Open the picture using image viewer  
<br>

### > Git Is Good - intelagent - 5pts
The flag used to be there. But then I redacted it. Good Luck. https://mega.nz/#!C483DAYB!Jjr55hfJQJ5-jspnyrnVtqBkMHGJrd6Nn_QqM7iXEuc 

**Solution:**  
You will find a text file inside the folder. If you open the file (flag.txt), you will find `flag{REDACTED}` string. Open the terminal  inside that folder. Type `git log` to view the commits history.
```
$ git log
commit d10f77c4e766705ab36c7f31dc47b0c5056666bb
Author: LaScalaLuke <lascala.luke@gmail.com>
Date:   Sun Oct 30 14:33:18 2016 -0400

    Edited files

commit 195dd65b9f5130d5f8a435c5995159d4d760741b
Author: LaScalaLuke <lascala.luke@gmail.com>
Date:   Sun Oct 30 14:32:44 2016 -0400

    Edited files

commit 6e824db5ef3b0fa2eb2350f63a9f0fdd9cc7b0bf
Author: LaScalaLuke <lascala.luke@gmail.com>
Date:   Sun Oct 30 14:32:11 2016 -0400

    edited files
```  
To revert to previous commit, you can use `git revert [commit SHA hash]`. After reverting, open the flag.txt file and you will find the right flag.  
<br>

### > Taking LS - alexkato29 - 2pts
Just take the Ls<br /> https://mega.nz/#!9Mk00LxR!_FtmAm8s_mpsHr7KWv8GYUzhbThNn0I8cHMBi4fJQp8<br /> NOTE: Problem is really only for mac users - sorry in advance - free points if you have a different operating system though :) 

**Solution:**  
You DON'T HAVE TO USE MAC to solve this problem. You can solve it using Linux :D. If you go to deepest "The Flag" directory, you will find a PDF file. That file can be opened if you have the password. There are two kind of files and folders, the visible and the hidden. The file or folder with '.' in front of the file name is hidden in Linux. So, to make it visible, you can use `ls` with additional flag behind it. Try `ls -ld .*` and you will see the output like this.

```
drwxr-xr-x 3 user rvm 4096 Okt 30  2016 .             # current directory
drwxr-xr-x 4 user rvm 4096 Jun 20 16:16 ..            # parent directory
-rw-r--r-- 1 user rvm 6148 Okt 30  2016 .DS_Store
drwxr-xr-x 2 user rvm 4096 Okt 30  2016 .ThePassword  # this is the directory you are looking for
```  
Go to ".ThePassword" directory and you will find a txt file which contains the PDF password. Enter the password to the PDF file and you will find the flag.


## Programming
Coming soon

## Binary Exploitation
Coming soon
