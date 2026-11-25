<h1>File permissions to enforce Least Privilege in Linux</h1>

<h2>Project Description</h2>
The Linux operating system allows cybersecurity professionals the ability to manage file permissions in an extremely easy and efficient manner using a command line interface. In this exercise, I will use the Bash Shell to review file and directory (folder) permissions. I will modify file permissions so that only appropriate users and groups have access to the files in a fictitious scenario wherein I am a security researcher in a research team at a large enterprise. I will also remove unauthorized permissions to these files and directories according to the principle of least privilege - users and groups should only have access and permissions to files and folders required for them to complete their own job tasks.

<h2>Check file and directory details</h2>
After opening the bash terminal, I use the <b>pwd</b> command (an abbreviation for "print working directory") to identify the directory in which I am currently working:

<p align="center">
<br/>
<img src="https://i.imgur.com/4mhf9p9.png" height="100%" width="100%" alt="Current working directory image"/>
</p>

I am in my user's home directory - the user I am logged in as is researcher2.

I want to however review files in another directory named <b>"projects,"</b> so I will need to change directories to get there.
I use the <b>cd</b> command (cd is an abbreviation for "change directory") so that I can view the permissions of files and sub-directories inside of the <b>projects</b> directory that the research_team group uses. Once in the projects directory, I issue the <b>ls -l</b> command to list out all of the non-hidden files and their permissions.

<p align="center">
<br/>
<img src="https://i.imgur.com/T61JkoG.png" height="100%" width="100%" alt="Non-hidden files and their permissions"/>
</p>

The <b>ls</b> command is used for listing files in a directory, and <b>-l</b> is added to view each file or sub-directory’s permissions.

However, since the directory also may include hidden files, I reissue the <b>ls -l</b> command, but this time, I add an <b>a</b> after <b>-l</b> to also view the permissions for any hidden files in the <b>projects</b> directory. So, that ends up being <b>ls -la</b> and I see that the directory also includes a hidden file <b>.project_x.txt</b>. You can tell that a file is a hidden file by the period at the beginning of a file name. 

<p align="center">
<br/>
<img src="https://i.imgur.com/95jt1d2.png" height="100%" width="100%" alt="Hidden Files image"/>
</p>

<h2>How to read the permissions string</h2>
The permissions for any given file or directory in the list is on the left of the file row string starting with either a <b>d</b> indicating the object is a directory or a - indicating the object is a file.

<p align="center">
<br/>
<img src="https://i.imgur.com/gP6olq0.png" height="100%" width="100%" alt="Permissions string image"/>
</p>

The following nine characters to the right of the d or - are the permissions for the user owner (u) of the file (first three rwx), the group owner (g) (second rwx), and other (o) (everyone/everything else)(third rwx). 

<pre><b>r=read     
w=write (and create files)   
x=execute (run executables and access the directory)</b></pre>

If there is a dash (-) in place of a letter, that means that the identity (u,g, or o) does not have the permission to perform that dashed out action. 

So rw-rw-rw- would mean all three identities have read and write permissions for the file/directory, but execute permissions.   

<p align="center">
<br/>
<img src="https://i.imgur.com/mOlOS43.png" height="100%" width="100%" alt="User owner and group owner"/>
</p>

<h2>Change file permissions</h2>
When the security professional identifies a permission that is unauthorized or that should be removed, or when they see that a user, group, or the public doesn't have permission to do something they should be able to do, we can easily change permissions in the bash shell. 

We do that with the <b>chmod</b> command (which stands for "change mode"). 

In the scenario below, I have identified a file, project_k.txt that had permissions allowing the whole world - "other" - to write to it.

<p align="center">
<br/>
<img src="https://i.imgur.com/Dxn2Dvk.png" height="100%" width="100%" alt="Other write permission image"/>
</p>

It has "w" visible in the last three-character block of the permissions string.  To remove that permission and deny the whole world from writing to this file, but still allowing the whole world to read the file, as well as continuing to allow the user owner and group owner to have read and write permissions to the file, I use the command <b>chmod o-w project_k.txt</b>.

<p align="center">
<br/>
<img src="https://i.imgur.com/1zlehm5.png" height="100%" width="100%" alt="Removing write permission from other"/>
</p>

To change a particular permission from a particular u,g, or o identity, we use  

<pre><b>- for remove   + for add   = for overwrite with (or apply)</b></pre>

My command <b>chmod o-w project_k.txt</b> changed the file's privileges to <b>rw-rw-r--</b>. The final <b>r--</b> shows that the write privilege was removed from <b>other's</b> (o) privileges for the file project_k.txt.

<p align="center">
<br/>
<img src="https://i.imgur.com/srzuh4y.png" height="100%" width="100%" alt="Other read privileges"/>
</p>

I also want to change the permissions to my file <b>project_m.txt</b>, which is a file that only the user researcher2 (me) should be able to access and read and write to. Right now, everyone in my research_team (the group owner) can read this file.

<p align="center">
<br/>
<img src="https://i.imgur.com/lkEUYNy.png" height="100%" width="100%" alt="Permissions on project_m.txt image"/>
</p>

I am going to remove their ability to read my project_m file by using the command <b>chmod g-r project_m.txt</b>

<p align="center">
<br/>
<img src="https://i.imgur.com/U6PYWHf.png" height="100%" width="100%" alt="Removing group privileges from project_m.txt image"/>
</p>

Now we can see that permissions for project_m only includes read (r) and write(w) for the user owner, me, and my research_team can't read, write, or execute the file, nor can "other", the rest of the world.

<p align="center">
<br/>
<img src="https://i.imgur.com/Gks7LJj.png" height="100%" width="100%" alt="Only user owner has permission to project_m.txt image"/>
</p>


<h2>Change file permissions on a hidden file</h2>

There is one more .txt file that needs permissions changed and that is the hidden file, .project_x.txt. 

<p align="center">
<br/>
<img src="https://i.imgur.com/NpOtYP3.png" height="100%" width="100%" alt="Hidden file .project_x.txt permissions"/>
</p>

<b>.project_x.txt</b> is hidden for a reason. It is a sensitive file and no one should be able to write to it and modify it, not even my user account researcher2. My security research team and I all do, however, need to be able to read this file. Right now, both my user owner account and the group owner account research_team have write (w) permissions for this file but the research team can't read (r) the file. 

To change that, I use the command <b>chmod u-w, g-w, g+r .project_x.txt</b> to remove write permissions from user and group, and add read permissions to group for this file.

<p align="center">
<br/>
<img src="https://i.imgur.com/2QvK2Ej.png" height="100%" width="100%" alt="Change permissions on .project_x.txt"/>
</p>

Now my user owner account researcher2 and my research team (group) have read-only permissions, and no one else can read, write, or execute the file.

<p align="center">
<br/>
<img src="https://i.imgur.com/uY7tFOD.png" height="100%" width="100%" alt="Resulting permissions for .project_x.txt image"/>
</p>

<h2>Change directory permissions</h2>

The last thing that I notice is that the drafts subdirectory (folder) in my projects directory (folder) shows that my research team can access this subdirectory - they have execute (x) permissions. 

<p align="center">
<br/>
<img src="https://i.imgur.com/rv4zuWc.png" height="100%" width="100%" alt="Group drafts folder permissions image"/>
</p>

On this particular folder, I should be the only one with the ability to access this folder, so I am going to remove my team's execute permissions using the command <b>chmod g-x drafts</b>

<p align="center">
<br/>
<img src="https://i.imgur.com/oP17TZV.png" height="100%" width="100%" alt="Changing permissions on drafts directory image"/>
</p>

Bye bye prying eyes! No one else but me can access at my draft folder!

<p align="center">
<br/>
<img src="https://i.imgur.com/AkrVs6s.png" height="100%" width="100%" alt="Resulting permissions on drafts directory image"/>
</p>

<h2>Summary</h2>
Understanding how Linux file permissions are modified is extremely important in the cybersecurity field so that we can ensure least privileges on our systems and investigate if any permissions changes have been made by malicious actors during intrusion incidents or persistent presence in our networks. Thankfully, their assignment and modification is very simple and the commands to interact with them are very straightforward.  

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
