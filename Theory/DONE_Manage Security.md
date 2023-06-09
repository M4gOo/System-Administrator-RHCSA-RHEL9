
configuring SSH servers and clients

configuration file
/etc/ssh/sshd_config      (sshd -> the D means daemon)   man sshd_config
/etc/ssh/ssh_config       (this is the config file for client, no D)


/etc/ssh/sshd_config
  can change the port number
  can change the AddressFamily, this can change for ipv4, ipv6 or both (look at man page and search for Family)
  can change ListernAddress, so you can specify a IP addr to listen
  can change PermitRootLogin, this case you can login as a root using ssh service
  can change the PasswordAuthentication, this case choosen no, only ssh keys it will be possible to login
  can change the x11forwarding - start a remote app and forwarding to you local machine to interact
  
For example disable passwordauthentication global, but if you want do an exemption for an user add below PasswordAuthentication
Match User <username>
      PasswordAuthentication yes
      
After change need to reload to apply the new changes
sudo systemctl reload sshd.service


For the client we can add a file under ~/.ssh/config, change the permission chmod 600 config
inside this file we can add host with informations
  Host centos
        Hostname <IP>
        Port 22
        User <username>
 
 then you can use only ssh centos  to access the host/server
 
 To use ssh keys, generate keys in the client
 ssh-keygen
 
 then copy the public key to the server
 ssh-copy-id username@IP
 
 to remove fingerprint from a specific server saved in the known host
 ssh-keygen -R <IP>
 
 when you login using ssh, the client has some default config file
 /etc/ss/ssh_config
 this is not a good practice to make change in this file because future upgrade of the 
 ssh client it might overwrite the changes, better way is to modify in 
 /etc/ssh/ssh_config.d/<name_of_conf_file>
 then you can add new port, for example
 
 
 List and Identify SELinux file and process contexts ======================================================================
 
 linux kernel can be easily extended with so-called modules, one of them is SELinux that add variant advanced
 capabilities of restricting access
 SELinux is enable by defaut by centos 
 
 ls -Z
 drwxr-xr-x. devops devops unconfined_u:object_r:user_home_t:s0 Desktop
 
unconfined_u:object_r:user_home_t:s0   ->string is called SElabel or SElinux context
unconfined_u - user
object_r     - role
user_home_t  - type
s0           - level

only certain users can enter certains roles and certain types
lets authorized users and processes do their job, by grating the permissions they need
authorized users and processes are allowed to take only a limited set of actions
everything else is denied

ps axZ


to see the security context assigned to the current user. What got applied to our user when we logged in
you can use:
id -Z

when we log in the user we log in as is automatically map to an SELinux user
To see this mapping is done
sudo semanage login -l
sudo semanage user -l  ->group

to see the SELinux is actively restricting actions 
getenforce
the output it must be Enforcing
if is Disabled is not doing anything
if it is Permissive means it is allowing everything and locking actions that should have been restricted intead of denying



Change kernel runtime parameters, persistent and non-persistent ===========================================================

kernel runtime parameters is what the linux kernel does its job internally
 kernel deals with low level stop like allocating memory, handling network traffic and so on. Most of those settings involve
 those types of things.
 To see all kernel runtime parameters currently in use
 sudo sysctl -a
 
 to change the values, for example ipv6.
 sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
 this change is not persistent
 
 to make a change persistent
 /etc/sysctl.d/*.conf
 
 to immediately set up the changes use
 sudo sysctl -p /etc/sysctl.d/<swap-less.conf>
 
 
 
 Restore default file contexts ==============================================================================================
 
 Using booleans values to modify settings of boot time, stop the boot process
 
 can add enforcing=0 ; this cause the system to boot into permissive mode for SELinux
  
  ![image](https://user-images.githubusercontent.com/57456345/218545692-50e026f4-6160-4db4-8238-73d68c8bad80.png)

  selinux=0 ; the kernel wont load anything to SELinux and the next time the system gets booted without that parameter
  it is going to force an auto relabel of the file system to get all the file context
  
  autorelabel=1 ; force an auto relabel
 
  
  
 
 
 
 
 
 
 
 
 
 
 
 
 
 

