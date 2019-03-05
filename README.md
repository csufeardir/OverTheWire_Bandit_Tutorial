# OverTheWire_Bandit_Tutorial
Tutorial for levels 0-26 of OverTheWire's The Bandit Game, with usage of Windows OS and PuTTY SSh client.

My aim is to explain every single term with details through the whole tutorial, which should create a decent basic knowledge about Linux OS and it's utilities. 

# Level 0
 The goal of first level is to connect the Bandit's game server via SSH. Now, the first question is, what is SSH? 
 
 <h3>SSH</h3>
 <p>
 SSH, in other words, Secure Shell protocol, is a way to securely connect the Shell of a computer, or server, as we're going to mention it with this name. What is this Shell? Shell is a program, that makes the connection between user input, and the OS itself. In Linux systems, this process is usually controlled by "Bash", a short for "Bourne Again Shell". Then what is this "Terminal"? Terminal is another program, that lets you interfere with Shell, and provides you a nice terminal-like GUI. 
  </p>
 
Too many terms, and too complicated? Okay, I will try to put it all simply:

[Our PC]  ----SSH---> [Server] / We create a connection between our computer and the server with SSH <br>
[Terminal] ----Shell----> [OS] / We send commands to Shell program, which is usually Bash, and it sends the commands to OS for us. With SSH, we don't interfere with Terminal, but while we're managing the server we can use it to access Shell. 

Now if everything is clear, we need to make this SSH connection! But how? First, we need an SSH client. A program to help us make this connection. I will be using PuTTY for Windows OS. You may use another client, or you can download it from the link below: <br>
https://www.putty.org/

![PuTTY Client with Bandit Settings](https://imgur.com/peCbPz3.png)

Now let's look at the informations we're given:
The server address we need to connect is: bandit.labs.overthewire.org
The port which will accept our SSH request is: 2220
Username and Password is: bandit0

Now first, I set the address and port number, and click "Open" on PuTTY. This opens up a shell connection to the server, which asks for username and password. After entering "bandit0" in both, I gain the access. 

[PuTTY Shell](https://i.imgur.com/sdjIMkY.png)

Finally, we set the connection, we can roam in the server freely! Now our goal is, to find the password which will give us access to Level 1, and it's somewhere in this server. To see what do we have in hand, I use the "ls" command, ls is a short for List, and it lists the files in the directory I'm in.
```
bandit0@bandit:~$ ls
readme
```
This is my output. There's a file named readme. Well, let's read it then? To read it, I need to use another utility: Cat! Yes, the cat. It's the short for "concatenate" and does many useful things. This time, we will use it to read what's inside this file:
```
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```
Voila! Here's our password for the next level.

Level 0 ---> Level 1 Password : boJ9jbbUNNfktd78OOpsqOltutMc3MY1
# Level 1

Not changing any setting in PuTTY, we establish SSH connection again, but this time, we use bandit1 as username, and the password we obtained from the last level as password.

I run the ls command once again, to see what I have in hand:
```
bandit0@bandit:~$ ls
-
```
Hmm, a file named with "-"? Okay, I know what to do, let me use Cat to read what's inside. 
Whoops.
Why doesn't it work? Well, you guess. Filename has dash in it, and when I give this filename as a parameter to Cat, it thinks I'm giving it a command line option. I must tell it that it's not an option, but a filename. Feeding it the full direction should do the trick:
```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
And here it is, our password for the next level.
Level 1 ---> Level 2 Password : CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

# Level 2

Once again, I establish the connection and run ls command in the home directory. 
```
bandit2@bandit:~$ ls
spaces in this filename
```
[SPACES? NOOOOOOOOOOOOOOOOOOOOOOOOOO](https://www.youtube.com/watch?v=rL81kD44C8E)

Now I have to tell Cat, that I don't mean different parameters by spaces, but the filename altogether. To do this, I use the power of quotes. 
```
bandit2@bandit:~$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```
And here we have it again.
Level 2 ---> Level 3 Password : UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

# Level 3

I go through the same routine, connection, ls... But this time, something's different. There's a file named "inhere", but it's marked in blue. What's with this strange file, why is it blue? I need more details. And these details, come with the right options. I will use de ls command, with the option "-long" or "-l" in short. This is going to give me more detailed information about this file.

[PuTTY Screenshot](https://imgur.com/OuyPXdO.png)

Now this simple line, contains decent amount of information, if you know how to read it. Let's start from right, and go to left:
inhere: This is our file's name.
Oct 16 14:00 : Last modification time of the file
4096 : File size in Bytes
root : Owner of this file (But why are there two??)
2 : Total number of files (Two??)
drwxr-xr-x : What is this???

Now, let's take it slowly. It all was going well, until we noticed root written twice, and then there's 2. What can these possibly mean? As you've probably guessed so far, this is no single file. This is a Directory file, or what you call a Folder. It's a file, which has files inside, so if you've tried to read it with Cat, you must have seen the error. Now "root root" and number 2 started making sense. But what's with those random letters? Let's go down to the rabbit's hole:

<h3> What is drwxr-xr-x ?</h3>
<p>Let's split this mysterious piece of text into 4 parts, shall we? First, the first letter: 
"D",
And then, the rest as trigrams:
"RWX",
"R-X",
"R-X".</p>

We seperated it into blocks, because that's how it should be read. The first block, first character, gives us the type of the file. "D", stands for Directory, and this is also why it's coded in blue text. We will see other file types in time as well, but let's continue. 

Rest of the characters, represent permissions to handle this file. First part, is the Owner's permissions. It says the owner is permitted to "RWX", which means owner can Read, Write or Execute this file. Second part, is Group permissions. Now, every user in Linux is assigned to a group, and these users' permissions are controlled through this group. "R-X" says the group users can Read, or Execute this file, but can't change it. The third and last part stands for Other, which literally means any other than owner and group users, and their permissions. 

On a side note: We can use "file [filename]" command in Linux, to see it's file type as well.

Now let's go on. Since we've figured that inhere is not a file to read, but a file to move into, we will use a new command:
"cd inhere", which literally means change directory to inhere. As we moved into the new directory, we run ls to see what's inside:
```
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$
```
I'm sorry, what? Nothing?

Well, sometimes you should look through your soul too see what your eyes can not, or sometimes.. You just have to add -a option to ls, to see the hidden files.

```
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Oct 16 14:00 .
drwxr-xr-x 3 root    root    4096 Oct 16 14:00 ..
-rw-r----- 1 bandit4 bandit3   33 Oct 16 14:00 .hidden
bandit3@bandit:~/inhere$
```

Please look carefully to the output, and the permissions line. Let's check the files we've found:
First one, named ".", and since it's permission line starts with "d", I know it's a directory file. More so, it's this file we're in right now.

Second is a directory file as well, with two dots? Guess what, it's a directory to home! You can check where they direct with "cd [filename]" command.

The third, and the one we're looking for: It's filetype is marked with "-" instead of "d". This means, this is not a directory, but a regular file. Also now I know, if a file's name starts with dot, such as ".hidden", this means that file is hidden and can't be seen without such options as we used. Now since it's a regular file, I can read it with Cat:
```
bandit3@bandit:~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
bandit3@bandit:~/inhere$
```
Here's our password.
Level 3 ---> Level 4 Password : pIwrPrtPN36QITSp3EQaw936yaFoFgAB

# Level 4

According to OverTheWire, the key is in one of the hidden files in /inhere directory. Let's give it a search:
```
bandit4@bandit:~/inhere$ ls -la
total 48
drwxr-xr-x 2 root    root    4096 Oct 16 14:00 .
drwxr-xr-x 3 root    root    4096 Oct 16 14:00 ..
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file00
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file01
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file02
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file03
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file04
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file05
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file06
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file07
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file08
-rw-r----- 1 bandit5 bandit4   33 Oct 16 14:00 -file09
bandit4@bandit:~/inhere$
```

There are 9 files in total, easy enough to bruteforce, but there's an easier way. Remember the sidenote before, the "file" command, that shows you the type of file? Let's run it:
```
bandit4@bandit:~/inhere$ file ./-file*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$

```
It's rather obvious, isn't it? :) Cat the file07 and get our password. 
Note: The syntax ./-file* means run on all the files, that start with "-file" and end with anything, due to asterisk(*).
Level 4 ---> Level 5 Password : koReBOKuIDDepwhWk7jZC0RTdopnAYKh

# Level 5

This time, we have 20 directories inside the inhere directory, and each one of them have more than one files inside. Now this is harder to bruteforce our way, but we have some extra clues this time, the file we're looking for is:

human-readable
1033 bytes in size
not executable

When we have this many files to search, and some specific properties to look for, I want to present you a tool to literally "bruteforce" it for us: Find! 

find command looks for files in a given directory, for given options. We will now use it for our search for the key.

```
bandit5@bandit:~/inhere$ find ./maybehere* -type f -size 1033c ! -executable
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7

```

As you see, It's been much easier. Let me explain how we used the find command: I give find command a directory, search all directories that start with name "maybehere", and end with anything. Give me all the files, that has a type f (regular, not directory), with 1033 bytes in size (bytes are symbolized with c) and not executable. I did not include the human readable part for simplicity's sake, but we did not need it either, while we have this many filters already.

Level 5 ---> Level 6 Password : DXjZPULLxYr17uwoI01bNLQbtFemEgo7

# Level 6

File we're looking for has following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size

A bit similar to the previous one.

```
bandit6@bandit:~$ ls -la
total 20
drwxr-xr-x  2 root root 4096 Oct 16 14:00 .
drwxr-xr-x 41 root root 4096 Oct 16 14:00 ..
-rw-r--r--  1 root root  220 May 15  2017 .bash_logout
-rw-r--r--  1 root root 3526 May 15  2017 .bashrc
-rw-r--r--  1 root root  675 May 15  2017 .profile
bandit6@bandit:~$
```
Let's do a similar search with find command, but this time, we will add the following options:
-size 33c // File should be 33 bytes in size
-user bandit7 // Bandit7 should be the owner user
-group bandit6 // Bandit6 should be the owner group

With these options, our command is:
```
find / -size 33c -user bandit7 -group bandit6
```

[PuTTY Screenshot](https://imgur.com/FysGHPd.png)

Eh? What is this output? While I'm searching through the whole directory, I also try to search for files I'm not authorized to access to. I still can read the output, but let's get rid of all those "Permission denied" errors.

I will add this line at the end of my find command:

```
2>/dev/null
```

So what does this line mean? It says redirect 2, to a file called null, which is inside /dev. What are all these?
Well, 2 is our code for error outputs. In an output stream, all system errors are denoted as 2. So what's with /dev/null? Null is a device that "drops" any data sent to it. Think of it as the garbage bin, we take all the system errors (2), and throw them into the bin (/dev/null). This gives us a clear output:
```
bandit6@bandit:~$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```

The rest is up to you. Just Cat the file and get the password.

Level 6 ---> Level 7 Password : HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

# Level 7










