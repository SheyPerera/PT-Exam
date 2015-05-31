# PT-Exam

##Question 1

A. The date (period): October 2, 2013

B. Pick a major botnet’s activity and list: United States, China, Japan

1. Source and Destination: Philippine

2. How long the attack has been occurring: 2 days

3. How has the attack been pulled off?: 

Philippine government websites hit by multiple DDoS attacks from Global Anonymous. Global collective hacking group Anonymous has started waging cyber-warfare against targets in the Philippines which mainly consist of government websites. Anon has started using DDoS (distributed denial of service) attacks on various government websites in an operation dubbed “#OpPhilippines” and took some of them down. One of their targets was Senator Tito Sotto’s webpage, which, as of press time, is loading slowly. Here are their other targets:



http://www.senate.gov.ph/

http://congress.gov.ph/

http://www.gov.ph/

http://president.gov.ph/

http://www.bir.gov.ph/

http://customs.gov.ph/

http://www.pnp.gov.ph

http://www.nbi.gov.ph

http://www.ntc.gov.ph/

http://www.doh.gov.ph

http://www.bsp.gov.ph/

http://www.marina.gov.ph/

http://www.dilg.gov.ph/

http://pia.gov.ph/

http://www.papt.org.ph

http://www.smokefree.doh.gov.ph/

http://www.mmda.gov.ph

http://www.meralco.com.ph/

http://www.doj.gov.ph/

http://titosotto.com/

They have been used a combination of botnets, HTTP floods and DDoS attacks to bring their targets down.
 
![27](https://cloud.githubusercontent.com/assets/12119233/7901023/121e1422-0792-11e5-8864-9b34d704cdfa.jpg)

![28](https://cloud.githubusercontent.com/assets/12119233/7901024/137111ee-0792-11e5-9f64-48d41489f4e7.jpg)

![29](https://cloud.githubusercontent.com/assets/12119233/7901025/15689e4a-0792-11e5-815b-eb50f1dca18a.jpg)

C. Which country(s) seemed to be generating the most botnet attacks

•	United States seems to be the most targeted country.
![31](https://cloud.githubusercontent.com/assets/12119233/7901031/736b57ee-0792-11e5-9ef0-555aa7fa0ce5.jpg)


![30](https://cloud.githubusercontent.com/assets/12119233/7901030/6bf045c4-0792-11e5-9aeb-e4b47babb37d.jpg)

•	China seems to be the most botnet attack generating country.
##QUestion 2

###Wargames:Leviathan

####Leviathan – Level 0

Leviathan’s levels are called leviathan0, leviathan1, etc. and can be accessed on leviathan.labs.overthewire.org through SSH. Data for the levels can be found in the homedirectories.
To login to the first level use:

Username: leviathan0

Passowrd: leviathan0

![1](https://cloud.githubusercontent.com/assets/12119233/7900843/312733b8-078c-11e5-9515-883abcb2253c.jpg)
![2](https://cloud.githubusercontent.com/assets/12119233/7900848/41b6e4a8-078c-11e5-8092-914dcb28c059.jpg)
![3](https://cloud.githubusercontent.com/assets/12119233/7900847/41b11fdc-078c-11e5-84e5-e67101beb88e.jpg)

Username: leviathan1

Password: rioGegei8m

![4](https://cloud.githubusercontent.com/assets/12119233/7900852/53203028-078c-11e5-9ab9-ee509849121f.jpg)

####Leviathan – Level 1

Leviathan 1 presents us with a binary in our home directory.  If we run it we see that is a utility that ask and checks for a password. Knowing this, the binary is either reading the correct password from somewhere else or has it stored within the binary. In this case, the password is stored within the binary. We need to trace the binary. Many of you might catch the small hint to the movie Hackers within the binary. When Plague states the 4 most common passwords are love, sex, secret, and god.

Username: leviathan2

Password: ougahZi8Ta

![5](https://cloud.githubusercontent.com/assets/12119233/7900853/542b4dc2-078c-11e5-8e9d-be3e48278115.jpg)

To see what functions it is calling let’s use “ltrace”.We can see it is using strcmp() to check if an entered password is the same as it's stored string, "sex". Running the binary again we can now try entering "sex" as the password to test what will happen.

![6](https://cloud.githubusercontent.com/assets/12119233/7900854/550b87f2-078c-11e5-8959-ffbd9cf4ce08.jpg)


####Leviathan – Level 2

Leviathan2 presents us with a small binary that belongs to the user leviathan3 and group leviathan2. The program contains a small security hole that can be exploited using a symbolic link.  To understand how the program functions at its core and what is happening behind the scenes when the program executes, we will use a few Linux commands and techniques to enlighten us with this information.

Username: leviathan3

Password:Ahdiemoo1j

When you run "printfile" we can see that it wants a file path argument. We are going to go ahead and create a directory and a file with a space in the name.

![7](https://cloud.githubusercontent.com/assets/12119233/7900855/57b1c1ec-078c-11e5-8ef1-dd851a6fe3de.jpg)
![8](https://cloud.githubusercontent.com/assets/12119233/7900856/58d0ac96-078c-11e5-8368-1a93008020a9.jpg)

So, what is happening here is a small security hole in how this program functions. We can see that the function access() and /bin/cat are being called on the file. What access() does is check permissions based on the process’ real user ID rather than the effective user ID.
While access does use the full file path, the cat on the file is not using the full file path. We can see this near the end of the output where /bin/cat/ is being called to tmp.txt as if it were a separate file, it’s really the second half of our filename. This can be exploited if we create a symbolic link from the password file to the file we created in /tmp.
Let’s create the symbolic link, but with only the first part of the filename we created before the space. Issue the command ls –al; we see file tmp.txt was not converted to a symlink, but a separate symlink called "file" was created.

![9](https://cloud.githubusercontent.com/assets/12119233/7900857/59c521fe-078c-11e5-88f6-fc54d2ef48ff.jpg)

Calling ~/printfile on the file that actually links to the password will fail. The file did actually access the symbolic link correctly up until file, but then tried to incorrectly cat the part of the file name after the space as if it were another file.

This all works because /printfile is owned by leviathan3. Access will call the symlink with that privilege. But we also needed to utilize a syntax hack to make it work, hence the filename with a space in it.

![10](https://cloud.githubusercontent.com/assets/12119233/7900858/5b912ffa-078c-11e5-9300-fc4dfc5e6a4c.jpg)

**ltrace - A library call tracer**

ltrace is a program that simply runs the specified command until it exits. It intercepts and records the dynamic library calls which are called by the executed process and the signals which are received by that process. It can also intercept and print the system calls executed by the program.
A symbolic link (also symlink or soft link) is a special type of file that contains a reference to another file or directory in the form of an absolute or relative path and that affects pathname resolution.


**A symbolic link** contains a text string that is automatically interpreted and followed by the operating system as a path to another file or directory. This other file or directory is called the "target". The symbolic link is a second file that exists independently of its target. If a symbolic link is deleted, its target remains unaffected. If a symbolic link points to a target, and sometime later that target is moved, renamed or deleted, the symbolic link is not automatically updated or deleted, but continues to exist and still points to the old target, now a non-existing location or file.
In POSIX-compliant operating systems, symbolic links are created with the symlink system call. The ln shell command normally uses the link system call, which creates a hard link. When the ln -s flag is specified, the symlink() system call is used instead, creating a symbolic link.

The following command creates a symbolic link at the command-line interface (shell):

    ln -s target_pathlink_path

target_path is the relative or absolute path to which the symbolic link should point. Usually the target will exist, although symbolic links may be created to non-existent targets. link_path is the path of the symbolic link.

**access()**

In some situations it is desirable to allow programs to access files or devices even if this is not possible with the permissions granted to the user. One possible solution is to set the setuid-bit of the program file. If such a program is started the effective user ID of the process is changed to that of the owner of the program file. So to allow write access to files like /etc/passwd, which normally can be written only by the super-user, the modifying program will have to be owned by root and the setuid-bit must be set. But beside the files the program is intended to change the user should not be allowed to access any file to which s/he would not have access anyway. The program therefore must explicitly check whether the user would have the necessary access to a file, before it reads or writes the file. To do this, use the function access, which checks for access permission based on the process’s real user ID rather than the effective user ID. (The setuid feature does not alter the real user ID, so it reflects the user who actually ran the program.)


    Function: int access (const char *filename, int how)

![11](https://cloud.githubusercontent.com/assets/12119233/7900862/82578a08-078c-11e5-9714-948f6ee3e18d.jpg)

####Leviathan – Level 3

For Leviathan 3, the process is fairly simple.  We only need a few commands that we are already familiar with to get access to Leviathan4’s shell. When we finally do get his shell, we can cat his password in /etc/leviathan_pass/leviathan4 and correctly log in as him to begin level 4->5. 

Username: leviathan4

Password: vuH0coox6m

![12](https://cloud.githubusercontent.com/assets/12119233/7900863/82c43716-078c-11e5-810d-56d5832dac61.jpg)
Let’s see if there are any interesting strings in it

![13](https://cloud.githubusercontent.com/assets/12119233/7900864/85ff4ede-078c-11e5-8008-530821dedfa9.jpg)

The point of interest for us will be the snlprintf word. Right after that you see the words, "You've got shell!” Let's use that as the password.

![14](https://cloud.githubusercontent.com/assets/12119233/7900865/86ef4394-078c-11e5-873d-5891a1738123.jpg)

All are familiar with snprintf() in C, but not snlprintf(). So we can assume it is a made up function or actually is the password. Clearly it is a visible string in the binary. If you follow the logic in the strings output, you can crudely tell this is what pops the shell in main.

![15](https://cloud.githubusercontent.com/assets/12119233/7900866/87df9754-078c-11e5-9696-849bff44cfc5.jpg)

####Leviathan – Level 4

Username: leviathan5

Password:Tith4cokei

![16](https://cloud.githubusercontent.com/assets/12119233/7900867/897603b4-078c-11e5-9d60-4172531180cc.jpg)

Covert binary values to ASCII and obtain the password. 

![17](https://cloud.githubusercontent.com/assets/12119233/7900869/8a140122-078c-11e5-92f0-a18b8b6ff8e4.jpg)

![18](https://cloud.githubusercontent.com/assets/12119233/7900870/8b355e8e-078c-11e5-90a3-318bfa25481a.jpg)

####Leviathan – Level 5

Username: leviathan6

Password: UgaoFee4li

In Leviathan 6, we will exploit a binary by using symbolic linking to a file we have permission of. When we run the binary in Leviathan5’s home directory, it appears to be attempting to read from a file in /tmp. The binary is owned by Leviathan6 but belongs to the Leviathan5‘s group. Therefore, it can pull Leviathan6’s password.We successfully exploited bad permissions placement on files via symlinking and now have Leviathan6’s pass as a result.

![19](https://cloud.githubusercontent.com/assets/12119233/7900871/8e605032-078c-11e5-8d24-b4dd0fc35006.jpg)

![20](https://cloud.githubusercontent.com/assets/12119233/7900873/91bd6c56-078c-11e5-9972-19f6469d5bd8.jpg)

####Leviathan – Level 6

Username: leviathan7

Password:ahy7MaeBo9

The final stage of Leviathan presents us with a problem that can be solved via some quick bash scripting. The binary in our home directory accepts a 4 digit combination as the password. Iterating through all the possible combination manually would take too much time. So we create a bash script to do this in a matter of seconds.



![21](https://cloud.githubusercontent.com/assets/12119233/7900874/936d4468-078c-11e5-8a7d-652c3906dce8.jpg)

![22](https://cloud.githubusercontent.com/assets/12119233/7900875/944da27e-078c-11e5-8289-f887e3a0a92e.jpg)

![23](https://cloud.githubusercontent.com/assets/12119233/7900876/95a2cc76-078c-11e5-926c-976c3bfbfb47.jpg)

Run the binary and simply wait for it to iterate through all possible number combinations. Note that we could echo out the password when we find it. Here the script will simply input the pass to the binary and pop us into leviathan7’s shell when it finds a match.

![24](https://cloud.githubusercontent.com/assets/12119233/7900877/9662268e-078c-11e5-8b34-30305c64c38a.jpg)

![25](https://cloud.githubusercontent.com/assets/12119233/7900879/9787bef2-078c-11e5-9255-ee13fbb0890f.jpg)

####Leviathan – Level 7
![26](https://cloud.githubusercontent.com/assets/12119233/7900880/9942569e-078c-11e5-9450-82736aa80f89.jpg)



##QUestion 3

###Question A 
a) /etc/passwd file is a text file that contains the basic information about each user or account running a linux Operating System.

a) /etc/shadow file stores actual password in encrypted format for user's account along with secure user account information such as additional properties related to the user password.

c) setuid bit is a unix access right flag that allow users to run an executable with the permission of the owner or group respectively and change the behavior in directories.

d) chroot is an operation in linux that changes the apparent root directory for the current running process and its children.

###Question B

lsattr command lists the file attributes on a second extended file system
ls -l shows the long listing information about the file or directory

###Question C
Android system implements the principle of least privilege. This means that each app, by default, has access to only the components that it requires to do its work and no more. The granularity of control that the OS has over privileges of an individual process is limited.
In practice it is very difficult to  control a process's access to memory, processing time, I/O device addresses or modes with the precision needed to facilitate only the precise set of privileges a process will require.

###Question D
Access control lists provides an additional, more flexible permission mechanism for file systems. ACL allows you to give permissions for any user or group to any disc resource. It is designed to assist with unix file permissions.
In standard unix permission models, the ownership of the files is an important component. Access to the files  and read, write, execute permission depends on the permissions given by the owner or group, 

###Question E
ruid is used to identify who the user is actually. once it is setup by the system, it cannot be changed till your session terminates. Users cannot change the ruid. only the root can change it.
euid is used to determine what level of access the current process has. when the euid is zero then the process has unlimited access

###Question F

In a unix system, Syslog file provides information about the activities of the machine, such as the user activity and history.IF the attacker clears this file, the illegal activities cannot be discovered. Need root login.

Altering accounting files in unix is another method of covering tracks. utmp, wtmp, and lastlog files are the main accounting files in unix. Need root login.


##Question 4


a) This command moves the value at memory location 0x8 to DWORD which is a 4 byte value in memory specified by the register EBP at the memory location 0x4
 
b) This command moves the value stored at DWORD which is a 4 byte value in memory specified by the register EBP at the memory location 0x8 to EAX register

c) lea loads a pointer to the value. Here it loads the address in the EAX and not the value. 

d) This calls the _htons function. The call puts the address of the next instruction on the stack so the program can return to it

e) This command compares the values of the two operands ebp+0x8 and 0. It actually checks whether the value at the 0x8 memory location of the EBP register is null.


##QUestion 5
