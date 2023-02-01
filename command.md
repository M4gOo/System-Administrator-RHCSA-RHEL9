
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
