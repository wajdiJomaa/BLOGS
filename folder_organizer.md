# folderOrganizer

## What is folder Organizer

The folder Organizer is a simple program built with C and some bash commands, by executing it inside a directory it will group the files sharing the same extension together in sub-directories.

##  THE CODE

### system

Before we start, you should know what the `system` function is in C, the system function takes a command, im my case on "UBUNUTU" it's a bash command, and execute it. For example system("mkdir /home/example/games"), creates a games directory in /home/example.

### generating a list of file names

The first step is generating a file containing a list of file names, but before this some files may have space in their names, and we don't want this to make problems,
so I found on stackoverflow a bash command to rename these files, and replace spaces with underscores "_",`"find . -type f -name \"* *\" -exec bash -c 'mv \"$0\" \"${0// /_}\"' {} \\;"`. Now we can generate the file using the command `ls > systemm`, ls will list all files in current directory and > will redirect it to a file called systemm.

note : all commands use a relative path to the directory of the program.

### One more problem before moving files

A directory may contain  subdirecories, so our file may contains name of other directories, so with the command `[-d name]` we can check for directories, this command returns 0 if the current name represents a directory, and 1 for files.

Another issues is that some files may have special characters, like "(", ")" , "," etc...
and when executing a command like `mv` on these files we should use `mv ex\,ample`
istead of `mv ex,ample`, so I created a simple function to add \ before these special characters to all filenames in the generated file.

### Getting extensions

The last thing before we start is defining a function to get the extension of a file, 
for this i found a function on internet that takes a filename and returns it's extension
`char * extension(char *filename)

### organizing and moving files

Now we can start moving files, we can open the file systemm containing the list of filenames using `fopen`, and do the following steps :

1. read a filename from the file using `fscanf`.
2. check if it represents a directory.
3. if not execute the function `slash_before_special_characters` on the file.
4. get the extension of the file.
5. check if the extension of file is not empty (some files has no extensions)
6. if it's not empty then move the file to a sufolder with the name of this extension, create this folder if it didn't exist. We can check for existence using ls.
7. if the file name is "systemm" or "organize" don't do anything, we don't want to move the file containing the list of file names and the program itself. It's safe to put this step in an else because these files has no extensions.
8. for other files without extensions move them to a folder called "no_ext"
9. repeat all steps until you finish.

## Finally...

I hope that I explained clearly how my code works.

## wajdi,18 Dec 2021




