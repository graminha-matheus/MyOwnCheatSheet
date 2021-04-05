# CHMOD Cheat Sheet

**Fonte**: https://tryhackme.com/room/linux2

chmod allows you to set the different permissions for a file, and 
control who can read it. The syntax of this command is typically `chmod <permissions> <file>` . 

The
 interesting part is how the permissions are set. They're set using a 
three digit number, where each digit controls a specific permission, 
meaning the first digit controls the permissions for a user, the second 
digit controls the permission for a group, the third digit controls 
permissions for everyone that's not a part of the user or group.  

Now
 the value of the digits control whether they can read, write or execute
 it or do all three, and to properly calculate it some math needs to be 
done.

| Digit | Meaning                                         |
| ----- | ----------------------------------------------- |
| 1     | That file can be executed                       |
| 2     | That file can be written to                     |
| 3     | That file can be executed and written to        |
| 4     | That file can be read                           |
| 5     | That file can be read and executed              |
| 6     | That file can be written to and read            |
| 7     | That file can be read, written to, and executed |

The way these values are calculated is this. The digit 1 means the file can
 be executed, the digit 2 means it can be written to, and the digit 4 
means it can be read. You get the different permissions by adding these 
digits together. For example 1+2 is 3 meaning that file can be executed 
and written to. Now let's see how it all works in perspective.

| Command:       | Meaning                                                                                                                                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| chmod 341 file | The file can be executed and written to by the user that owns the fileThe file can be read by the group that owns the fileThe file can be executed by everyone else.                                                           |
| chmod 777 file | The file can be read, written to, and executed by the user that owns the fileThe file can be read, written to, and executed by the group that owns the fileThe file can be read, written to, and executed by everyone else<br> |
| chmod 455      | The file can be read by the user that owns the fileThe file can be read and executed by the group that owns the fileThe file can be read to and executed by everyone else                                                      |

ls provides a helpful way of viewing the permissions of files in the current directory.

![](https://imgur.com/MPSlodl.jpg)

Recall
 that file permissions are divided into three sections, user and group 
and everyone else. The same is true here; however, everything starts 
from the second hyphen not the first, so we can just forget the first 
hyphen for now. Note that everything is in sequential order, so the 
first three characters control permissions for the user, the second 
three characters control permissions for the group, and the final three 
characters control permissions for everyone else

![](https://imgur.com/ZNaY6Iw.jpg)  

(Forgive the artist's rendition. U = user, G = group, E = everyone else)

rw
 means as you might expect "read and write", meaning the user has read 
write perms to the file. Following that logic, that means members of the
 group and everyone else have only read perms. To convert that to 
numbers the permissions for that file in number form are 644. We can 
test this by trying to change the permissions

![](https://imgur.com/hu9mkJC.jpg)  

When
 we try to change the perms to 644 nothing happens because the perms are
 already 644. The interesting part is while we can write data to 
.profile with echo while the perms are 644, we can't when we change the 
perms to 544, because we took away our own write perms. Following that 
logic, that means we can completely lock ourselves out of writing to a 
file we already own!  

Note:
 It is possible to give someone no perms to a file, You can just put 0 
as the digit. 770 Means that everyone that isnt a part of the user or 
group cant do anything to the file.
