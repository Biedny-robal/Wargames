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
Here the whole level is solved using the "find" command, as following:
```
find -type f -size 1033c | xargs cat | head -c 32
```
after we find the file, we take the first 32 characters, which are the password
### Level 6
Same as before, we have to use the find command. This time, there will be some permission errors, this is expected.
```
find / -group bandit6 -user bandit7 -size 33c | xargs cat
```
### Level 7
The password is stored in the file data.txt next to the word millionth, we use the grep command to get the line with the password, and extract it using tail (end of line):
```
grep -e "millionth" data.txt | tail -c 33
```
### Level 8
The password is stored in the only non-repeating line of the data.txt file.

We start by sorting the lines in the file, then passing it to the uniq function that counts how many times each line occurs, filtering to only 1 occurence using grep, and finally taking only the password using tail;
```
sort data.txt | uniq -c | grep '1 '|tail -c 33
```
### Level 9
The password is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

We check for human readability using "strings" then pass to grep to check for more than 1 equal sign;
```
strings -a data.txt | grep -E ^==+
```
### Level 10
this time the password is written in plain text, but file is base64 encoded:
```
base64 -d data.txt | tail -c 33
```
### Level 11
The password is in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

To decode we will substitude using tr. We need to first cat, to pipe the contents of the file into tr, and then we use tail to extract the password.
```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m' | tail -c 33
```