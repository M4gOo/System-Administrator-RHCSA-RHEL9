
id  uid,gid,...

who  users log in the current system

last  show the logged in users

su -  switch users, this requires the password for the user you are switching to

sudo  better option for su, prompt to type the user is logged in. Default conf (/etc/sudoers), this can be update using visudo

          under /etc/sudoers  for users to run specific commands: user localhost=/usr/bin/shutdown -r now
                              can set up shell /usr/bin/nologin -> this means noone can login into the system using root user and password directly
                              
                                                      
     
