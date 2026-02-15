### Logging into each level
Logging into each level goes the same way, we use the command:
```
ssh -l banditx -p 2220 bandit.labs.overthewire.org
```
(where we replace x with the level)

and then use the password from the previous level. for level 0 the password is "bandit0"

Finish each level by logging out, so that you can log into the next one. You can do this using:
```
exit
```
### Level 0
in order to advance to the next level, we fetch the password from the "readme" file, using:
```
cat readme
```
alternatively, to get just the password (assumed to be 32 characters long), we can use:
```
tail -c 34 readme | head -c 32
```
this takes the last 34 bites of the file (password + newline + EOF) and then takes the first 32 bites of the result - which is the password.
### Level 1
The password is in the file named -
Here we can't use the simplest solution of "cat" because "cat -" doesn't return the file. We need to specify the file by name using quotation marks, as such:
```
cat ./"-"
```
### Level 2
This time the password is in a file that has spaces in it's name, the solution is identical to level 1:
```
cat ./"--spaces in this filename--"
```
### Level 3
The password is in a hidden (name beginning with ".") file in the directory "./inhere".

We enter the directory, find the hidden file, and print it's contents:
```
cd ./inhere && ls -A ./ | xargs cat
```
&& executes the command on the right after the left one succeds
-A has ls list hidden files (but not . and ..)
xargs makes sure cat interprets the input as a filename and not text.
### Level 4
This time we have a number of files in a directory, and the password is in the only human readable file.

We list all the files with their types, find the file that has type "ASCII text" and then print it's contents:
```
file ./inhere/* | grep -e 'ASCII text' | head -c 16 | xargs cat
```
here we are assuming that we know the full name of the file is exactly 16 bytes, this can change, but accounting for it in a one line seems pretty annoying, so I'll skip it, doing it in two lines is trivial.
### Level 5