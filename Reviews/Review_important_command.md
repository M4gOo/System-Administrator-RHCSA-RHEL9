
id  uid,gid,...

who  users log in the current system

last  show the logged in users

su -  switch users, this requires the password for the user you are switching to

sudo  better option for su, prompt to type the user is logged in. Default conf (/etc/sudoers), this can be update using visudo

under /etc/sudoers  for users to run specific commands: user localhost=/usr/bin/shutdown -r now
                    can set up shell /usr/bin/nologin -> this means noone can login into the system using root user and password directly
                              
   the number 3 in brackets is user:group that first field can run the command
   
  ![red1](https://user-images.githubusercontent.com/57456345/216167861-f64a590b-6cc1-47c0-b387-4089bcd38de0.JPG)


 info <command>, --help; man; man man; man 1 printf, apropos (in case I forgot the name of the command, search in the man pages; in case it is new VM run sudo mandb)
 apropos -s 1,8 <wildcard>
  check manuals programs packages installed on the system /usr/share/doc

  ls -alh 
  shows bytes of the files
  
 absolute path always start with the /
  relative example you are /home/user and want to move to /var/log -> cd /var/log
  
  when copy it is a good practice to terminate with / whne it is directory
  cp file.txt dir/
  cp file.txt dir ->this dir could be a file
  cp -r (recursive)
  cp -r Copy/ Paste/  with the paste dir already exist it will be under Paste/ this Paste/Copy/files and dir
                      with the paste dir doesnt exist it will be under Paste/ this Paste/files and dir
  when use move command mv, doesnt need to use the flag -r
  
  rm -r to delete directories
  
  
 mkdir and subdirectories ( -p flag)
  mkdir -p /tmp/1/2/3/4/5/6/7/8/9
  
  
============= List, set, and change standard ugo/rwx permissions

  can only change to a group that the user belongs to
  change group
  chgrp <group_name> dir/file
  
  groups that user is part of
  groups
  
  change owner
  chown
  sudo chown <user> <file/dir>
  
  to change group and owner at same time
  sudo chown user:group <file/dir>
  
  when directories has R means they can be listed; W can create files/subdir; X you be able to use cd
  
 the system linux allow permitions from left to right
    if even the owner has only read permissions and the user belongs to a group which can read and write. The user wouldnt be able to write in the dir/file
      -r--rw----
    but if another user who belong the group, login and try  to write it will be able to do that
  
  to change permissions
  chmod <permissions> file/dir
  
  
  SUID, SGID, and sticky bit
suid when is set that whenever the file is executed it is going to be executed as the user ID of the owner of the file
  chmod 4664 file.txt  -> -rwSrw-r---   file.txt  means only the suid are set and the X (execute permission) not set
  chmod 4764 file.txt  -> -rwsrw-r---   file.txt means the X and the suid are set
  
  SGID ----------
  chmod 2664 file.txt  -> -rw-rwSr--- 
  
  SUID + SGID
    chmod 6664 file.txt  -> -rwSrwSr--- 
  
  find files which has suid or sgid
  find / -perm /6000    -> it will find all suid, sgid and suid+sgid
  find / -perm /4000
  find / -perm /2000
  
  sticky bit
  set on dir that are shared between people. This block other users to delete or rename dir that doesn't belong to them even they have permissions to delete, change names in your profile
   chmod +t dir
  chmod 1777 dir    drwxrwxrwt dir
  
  
  ================== FIND ==================
  
  find /path -name file.txt
  find /path -iname file.txt  -case sensitive
  find -mmin        modified minute (modified means creation or editing a file) Modified means when the contens have been modified
     find -mmin 5     5 min ago (if now is 11:00 it will find files in 10:55)
    find -mmin -5     5 min ago (if now is 11:00 it will find files BETWEEN 10:55 until 11:00)
  find -mmin +5     list files before 5 min ago (if now is 11:00 it will find files from 10:55 to the infinite 10:45, 10:30, 9:30, etc)
  
  find -mtime 0     search for files per days or past-24hours periods
  0 past 24 hours
  1 24h to 48h
  
  modified
  find -cmin -5   modified last 5 minutes
  
  find -size
  c bytes
  k kilobytes
  M megabytes
  G gigabytes
  
  find -size 512k       = 512
  find -size +512k      > 512
  find -size -512k      < 512
                             Find all files between 5mb and 10mb in the /usr directory
                             sudo find /usr -type f -size +5M -size -10M
             
   find -perm 664       exactly perm
   find -perm -664       find bearly perm  
   find -perm -u=rw,g=rw,o=r       find bearly perm                          
   find -perm /664       find files with any of these perm                          
                             
  find -name "f*" -size 512k      AND operator                     
  find -name "f*" -o -size 512k      OR operator  
  find -not -name "f*"              NOT operator
 
  Find files/directories under /var/log/ directory that the group can write to, but others cannot read or write to
   sudo find /var/log/ -perm -g=w ! -perm /o=rw                          
  
  
 find file
  find /path -type f                         
  
 
  
  =========================  Compare and manipulate file content =======================================
   tac file.txt   -> will see bottom to top  inside the file which is small
  
  cat file.txt    -> top to bottom inside the file which is small
  
  tail file.txt   ->  will see bottom to top  inside the file which is large, default is 10 lines
  
  tail -n 20 file.txt ->  will see bottom to top  inside the file which is large, shows 20 lines
  
  head file.txt   ->  will see top to bottom  inside the file which is large, default is 10 lines
  
  head -n 20 file.txt ->  will see top to bottom   inside the file which is large, shows 20 lines
  
  head -5 file.txt  ->  will see top to bottom   inside the file which is large, shows 5 lines
  
  
  sed 's/old_string/new_string/g' file.txt
  
  s -> substitute
  
  g -> global replace
  
   sed -i 's/old_string/new_string/g' file.txt
  
  -i -> will edit the file, it will swap the strings
  
  sed -i 's/disabled/enabled/gi'
  
  gi - ignore case sensitive
  
  Change all values enabled to disabled only from line number 500 to 2000.
  
   sed -i '500,2000s/enabled/disabled/g' file
  
  Replace all occurrence of string #%$2jh//238720//31223 with $2//23872031223 i
  
  sed -i 's~#%$2jh//238720//31223~$2//23872031223~g' file
  
  
  extract the name alone in the file
  
  ![image](https://user-images.githubusercontent.com/57456345/216945745-dc02349a-2708-4bd5-a53a-ff23b653bc9d.png)

  cut -d ' ' -f 1 file.txt    first column that appears on each line
  
  output: ravi
          mark
          jon
          ravi
          mary
  
  d -> delimiter
  
  ' ' -> words are separated by space, ',' words separated by comma
  
  f -> fields we want extract
  
  
  when you have duplicates words in a file, but want only once the word. uniq remove the adjacent,for that we need to use sort to help this command
  sort text.txt | uniq 

  
  analyze files, see the differences between them 
 diff file1 file2
  
 diff -c file1 file2 
  
 diff -y file1 file2  (side by side)
  
 sdiff -y file1 file2  (side by side)  
  
  
 LESS command has more features than MORE command
  less /var/log/text.log     (arrow keys for up and down; search function /string  then press N for next instance --i for sensitive  go back instance shift+N)    
  
  more /var/log/text.log   (use space to move pages)
  
  vim text.txt
  use /search_string\c     \c case sensitive   
  to coppy the line yy and paste p
  to cut is dd
  
  
  ==================== GREP ===========================
  
  grep -i 'string' /path-file   i case sensitive
  
  grep -r 'string' /path         r recursive
  
  grep -w 'string' /path         w match the only word
  
  grep -o 'string' /path         o output it will show only the word
  
  
  
  ================== Analyze text using basic regular expressions  ================================================
  
  https://regexr.com/
  https://regexr.com/
  
  
  ![image](https://user-images.githubusercontent.com/57456345/216956502-76314052-048e-42f0-b610-242cafc1136a.png)

  ![image](https://user-images.githubusercontent.com/57456345/216956665-04e642ec-ca3b-41ec-bec9-235e944bdcdc.png)

  ![image](https://user-images.githubusercontent.com/57456345/216956874-bff42496-cf91-4090-9fff-cb1346826c21.png)

  . it will match any one character; can use -wr for full word and not partil word, to match a DOT use \. (this is be a regular period)
  
  ![image](https://user-images.githubusercontent.com/57456345/216957734-4e5ded0d-1234-41e4-888b-f63a7711f8f9.png)

  ![image](https://user-images.githubusercontent.com/57456345/216957893-6808a477-c73d-4d49-8656-c55dc95c52e4.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/216958105-7c960a30-be35-4b06-8a15-fe3a9be550ff.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/216958391-b90a531e-8721-4bfa-8677-e96b6e7b123a.png)
  
  need to use plus symbol to avoid these output, the * lets previous character exist one or more times, but also 0 times
  
  ![image](https://user-images.githubusercontent.com/57456345/216958629-5ad6c84b-10b1-4099-92ba-66351f96ce8a.png)
  
  to avoid to use \ and get confuse with \. and so on. It is good practice to use grep -E or egrep

  ![image](https://user-images.githubusercontent.com/57456345/216959006-eacaeddc-1204-454c-a58c-c4beedc667bf.png)

  ![image](https://user-images.githubusercontent.com/57456345/216959478-d6c24dd7-8370-4b4b-991d-91a310010dfa.png)

  to find at least 3 zeros  egrep -r '0{3,}' /path/ 
  
  exactly 3 zeros           egrep -r '0{3}' /path/ 
  
  range of zeros           egrep -r '0{3,5}' /path/ 
  
  ![image](https://user-images.githubusercontent.com/57456345/216960615-f9a9d734-519e-4806-817d-dd4b76bafa4a.png)
  
  match one thing or the other 
  
  ![image](https://user-images.githubusercontent.com/57456345/216960969-14b6608b-04ad-4ec9-9400-fff1920de129.png)

  ![image](https://user-images.githubusercontent.com/57456345/216961104-6e90e5ff-b0de-4c25-bbfa-ec40be1c8de9.png)

  Ranges or Sets []
  [a-z] [0-9] - ranges
  [abz954]    - sets
  
  ![image](https://user-images.githubusercontent.com/57456345/216961472-f7992cc3-1345-4af3-990b-66d7d76116b7.png)

  ()   
  
  ![image](https://user-images.githubusercontent.com/57456345/216962486-14c135b4-8a71-457d-be84-b7a7e9b56229.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/216962535-7fc7afb2-f101-4282-a774-a01c4f8de126.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/216962984-d88e3106-90de-43b2-88e4-9934cc9778c1.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/216963365-b01debf3-d9df-422a-933e-488f98b1dfcb.png)

  
  
==================== input-output redirection =======================

  ![image](https://user-images.githubusercontent.com/57456345/217201925-f2230aef-20ed-41b8-8abc-8ade0c0faaff.png)

  
if you want to save the output with the errors as well
  
 ![image](https://user-images.githubusercontent.com/57456345/217202171-15a00c7f-f145-4d70-8bed-2d1322065744.png)
  
  ![image](https://user-images.githubusercontent.com/57456345/217207367-b2d23284-4c74-4d11-9d2b-fed0208326c4.png)

  this is good for shell script <<<
  
  ![image](https://user-images.githubusercontent.com/57456345/217207501-52ec20ab-3397-465f-a28e-aa77f98aa2aa.png)

   ![image](https://user-images.githubusercontent.com/57456345/217209347-9a82fb44-29e1-4e9c-8195-d3a0a71394b7.png)

                                                                                                                                                  
                                                                                                                                                  
                                                                                                                                                  
  
  

  
  
  


  

  
  
  
  
  
