# Linux Cheat Sheet 2

From "TryHackMe" Linux Fundamentals.

## "ln" command

ln is a weird one, because it has two different main uses. One of 
those is what's known as "hard linking", which completely duplicates the
file, and links the duplicate to the original copy. Meaning What ever 
is done to the created link, is also done to the original file. The ln 
syntax is `ln source destination`.  

![](https://imgur.com/GFFFO2u.jpg)

It's important to be very careful with hard links, as depending on what 
you're doing it can be very easy to erase data from a file.  

The next form of linking is symbolic linking(symlink). While a hard linked 
file contains the data in the original file, a symbolic link is just a 
glorified reference. Meaning that the actual symbolic link has no data 
in it at all, it's just a reference to another file.  

The syntax for a symbolic link is the exact same, but it uses the -s flag, so to create a symbolic link, you would run `ln -s <file> <destination>`.

![](https://imgur.com/9E6e92K.jpg)  

ls even shows that its a symbolic link with the arrow pointing to what the
 link is referencing. It is important to note the permissions on the 
symlink. It has full 777 perms meaning that in theory you can execute 
the symlink, however since it is just a reference, in reality it has the
same perms as the original file.  

![](https://imgur.com/7r8Jmow.jpg)

---

## "find" command

find is an incredibly powerful, but incredibly simple command. It allows you to do 
just as it says, find files. It does this by listing every file in the 
current directory, so if you ran find /tmp it would list every file in 
/tmp. 

![](https://imgur.com/3zcvXKh.jpg)

It is worth noting that find is recursive, so it searches every directory 
that is in the original directory you provided. Meaning that if you were
 to run `find /`, it would list every file on the OS. Another thing worth noting is that it 
can only list files in directories that you have permission to access, meaning you cant just list every file as every user.  

The true power of this command though comes from the parameters you can provide it. You can use `find dir -user` , to list every file owned by a specific user; you can use `find dir -group` to list every file owned by a specific group. The sheer customizability of the command is it's most powerful feature.  

![](https://imgur.com/Ckqt2ax.jpg)

This is one command I highly recommend reading the manual page on to learn 
all of it's options. This command is invaluable when working with files.

![](C:\Users\Graminha\AppData\Roaming\marktext\images\2021-03-25-10-33-00-image.png)

---

## "grep" command

I can say without reservation that grep is one of the most useful 
commands to learn. It allows you find data inside of data. When working 
with large files, or a large output, it is arguably the best way to 
narrow the output down to better find what your looking for. The syntax 
of the command is `grep <string> <file>` however file is optional if you're using piping.

Note: You can search multiple files at the same time, meaning you can theoretically do `grep <string> <file> <file2>`  

For instance, let's say you know have the file name of test1234, but you 
don't know where it is on the system. find can be used to list every 
file on the OS, and grep can be used to find the actual file.

![](https://imgur.com/IR7M08S.jpg)

grep comes and saves the day! What about if you have a bunch of data, and 
you wanna see if the string hello is in it, and if so what line number 
it's at.

![](https://imgur.com/xPWWXQe.jpg)

I'm sure you can see just how useful this command is. When searching logs 
for the cause of an error message, when parsing large amounts of data 
for that specific piece, when searching every file in a directory for 
that one line that you may need to change.

Another important thing to note is that grep supports regular expressions, you 
see I wasn't being entirely truthful with you(props if you get the 
reference) when I says the syntax is `grep <string> <file>`, the syntax is actually `grep <regular expression> <file>`. Unfortunately regular expressions are out of the scope of this room,  but I highly encourage you to read up on regular expressions, as they increase the power of grep tenfold.

![](C:\Users\Graminha\AppData\Roaming\marktext\images\2021-03-25-10-40-21-image.png)

---

## "sudo" command

Throughout this room you might have seen me mention the root user. 
The root user is the equivalent of the administrator user on Windows, and like Windows certain commands, and certain things you download from the internet will require admin permissions.

That's where sudo comes in. sudo is Linux's run as administrator button, and the syntax goes `sudo <command>`.

![](https://imgur.com/ejD2Dib.jpg)  

Note: whoami is just a command that states your current user.

As you can see when using sudo the command is run as root. It is important to note that you need to have your current user's password to use it. 
Again like Windows, not every user has permission to use sudo, but most 
Linux OS' set up a user that has permissions when you install it.   

Assuminyou create a new user that you also want to give sudo permissions to, 
the man page for sudo has a section on how to add user permissions. It is also worth noting you can configure sudo to run commands as other users, again the man page has a section on that (sudo has a very nice man page).

![](C:\Users\Graminha\AppData\Roaming\marktext\images\2021-03-25-12-07-15-image.png)

---

## Adding users and groups

You know about how to modify permissions for users and groups, therefore it's 
helpful to know how to create them. Luckily Linux provides a nice helpful way to do this, with `adduser` and `addgroup`. The syntax for both of these commands are`adduser username` and `addgroup groupname`.

![](https://imgur.com/wWwteJA.jpg)

![](https://imgur.com/sEWC06K.jpg)  

It's important to note that only root has permissions to add users and groups, as seen with the failure when I attempted to run the commands without sudo. You may be wondering how to add a user to a group. that is done with the usermod command, the syntax for that is `usermod -a -G <groups seperated by commas> <user>`. Meaning if I wanted to add the user noot to b I would run `usermod -a -G b noot`.

![](https://imgur.com/vmxNa4e.jpg)

Note: id is a command that allows you to view basic information about a user.

---

## nano

Up until this point you may have seen me only using >> to add content to a 
file. Luckily that's not the only way to do things, nano is a terminal based text editor. The syntax for nano is `nano <file you want to write to>`. For example typing nano test will take you to this screen.

![](https://imgur.com/s0uJCNT.png)

From here type until your heart's content!

![](https://imgur.com/Kj5VuJW.png)

Nano
 has alot of commands inside of it's text editor, and the text editor as
 a whole probably warrants a room on it's own, but 99.9 percent of the 
time you're gonna wanna use `ctrl+x`

Note: ^X means ctrl+x, most of the time you see ^<key> the ^ means control

Once you press ctrl +x you'll be prompted with this screen

![](https://imgur.com/XqkoQbk.png)

From there press Y and you'll be asked what you want to name the file.

![](https://imgur.com/ao053v7.png)

Type
 whatever you want the file to be named, press enter and you'll be 
greeted with your terminal screen! You'll notice the file is there and 
everything you typed is there.

![](https://imgur.com/xlMG3H7.png)

Now you can actually edit text files! :)

---

## Basic Shell Scripting

Linux provides us a way to run commands one after another without using any special operators. This is done by storing the commands you want to run in a file with a .sh extension

![](https://imgur.com/HODFyrK.png)

If we save and run `bash s.sh` it executes those commands in order.

![](https://imgur.com/EGB71ea.png)

It echoed out hello, then it echoed out whoami, then it ran whoami exactly as the file said it should. Congratulations that's your first bash script!

It is worth noting that the sh extension isn't technically needed if you provide a shebang(#!) , and then the path to the shell we want to use to run our command at.

![](https://imgur.com/a5AX8U4.png)

From there we can remove the sh extension, and make the file executable.

![](https://imgur.com/UH9LQkO.png)

Note: The shebang MUST be in the beginning of the file

---

## Important Files and Directories

Throughout this room you've seen a lot of files and directories, so I'm using this task to define what the most important of those files and directories do.

Before that however, it is worth noting exactly how the linux file system works. Everything on the linux file system extends from "/". / is the equivalent of C: in Windows. Meaning that for example if you were to delete "/", you would delete every file on your system.  

**/etc/passwd - Stores user information - Often used to see all the users on a system**  

    ![](https://imgur.com/EhQNy3H.png)

**/etc/shadow - Has all the passwords of these users**  

    Not showing for obvious reasons

**/tmp - Every file inside it gets deleted upon shutdown - used for temporary files**

    ![](https://imgur.com/SEtC1rs.png)

**/etc/sudoers - Used to control the sudo permissions of every user on the system -**  

     https://imgur.com/SEtC1rsToo Long to show

**/home - The directory where all your downloads, documents etc are. - The equivalent on Windows is** C:\Users\<user>

    ![](https://imgur.com/YQnZLbJ.png)

**/root - The root user's home directory - The equivilent on Windows is** C:\Users\Administrator

       Not showing because of spoiler ;)

**/usr - Where all your software is installed** 

        ![](https://imgur.com/e0l4Ybh.png)  

https://imgur.com/e0l4Ybh**/bin and /sbin - Used for system critical files - DO NOT DELETE**

Too big to show, but contains all of the basic programs needed for linux to function

**/var - The Linux miscellaneous directory, a myriad of processes store data in /var**  

    ![](https://imgur.com/tyIiCeg.png)

**$PATH - Stores all the binaries you're able to run**

$PATH is an environment variable that contains all the binaries you're able to execute. 

![](https://imgur.com/vQYYnlr.png)

It is worth noting that the paths in $PATH(hah!) are separated by colons. Every executable file that is in any of those paths you are able to run just by typing the name of the executable instead of the full path.

![](https://imgur.com/8csjrPX.png)  Note: which is just a command that shows you where an executable is in any of the PATH directories.

---

## Installing packages with "apt"

This is a  bit hard to make a task on because depending on the Linux OS you 
install, this information may be entirely worthless. Therefore, I have deemed it best to show how to install packages using the most popular package manager, that being apt. A package is essentially a program, you can think of it like an exe file on windows. To install packages you need root permissions, as each package will likely modify some system critical directories such as /usr. 

The syntax to install packages is `apt install package`.

![](https://imgur.com/VaGFf6H.png)

Note: python-dev is a random package and just the first one that came to mind apt downloads the package from a repository(list of programs). It then installs it and gives you your command prompt back, much like when you install a program on Windows.

![](https://imgur.com/OfsAQ6N.png)

apt has a lot of sub commands, and again warrants a room on it's own but most of the time you're going to be googling what you want and you'll find the name of the package to install.

---

## Processes

Every binary you execute on linux, is a process while it's run. A process is just another word for a running program. A list of user created processes can be viewed with the `ps` command

![](https://imgur.com/pP9hnen.png)

While this is technically all the user created processes, it's likely not the information you're looking for if you're going to be examining processes. To view a list of all system processes, you have to use the `-ef` flag

![](https://imgur.com/WiLQ5Rq.png)

Every process that is currently running on the system is listed, along with some basic information about the process. Arguably the most interesting part about that list is the second column, the 3-5 digit numbers. Those are known as Process ID's(PID's) and they're how you interact with the processes. 90% of the time you'll likely be wanting to stop these processes and that's done with the `kill` command (an apt choice of naming I know). 

The syntax of kill is `kill <PID>`.

![](https://imgur.com/o8u8nhh.png)

![](https://imgur.com/UKq8sEN.png)

After running kill, the process no longer shows up! Another useful way to interact with PID's is through the command top. top shows you what processes are taking up the most system resources, which allows you to manage the resource allocation on your system by killing unneeded processes.

![](https://imgur.com/E3OeMWn.png)

Note: The top man page has descriptions for what every value means, and how they affect your system; I highly recommend reading it!
