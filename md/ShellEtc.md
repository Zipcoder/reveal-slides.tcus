#Shell, etc.


-
-

## What we'll cover

<p class="fragment fade-up">Your Shell Terminal</p>
<p class="fragment fade-up">Useful commands for navigating and manipulating your file system</p>

-
-
## Useful Commands
<p class="fragment fade-up">**w** - Who's logged on?</p>
<p class="fragment fade-up">**pwd** - "Print Working Directory" prints out your current location (known as your current working directory or 'cwd')</p>
<p class="fragment fade-up">**ls** - lists contents of the cwd</p>
<p class="fragment fade-up">**cd** - change directory. This is how you move around the file system. You can specify the destination as an absolute or relative path.</p>
<p class="fragment fade-up">**echo** - prints text to the terminal</p>
<p class="fragment fade-up">**cat** - concatenates zero or more files. Often used to print the contents of a file to the terminal. Often misused</p>


-
-
## Useful Commands (contin.)
<p class="fragment fade-up">**less/more** - display contents of a file one page at a time</p>
<p class="fragment fade-up">**grep** - search for the specified text or pattern</p>
<p class="fragment fade-up">**touch** - create or open a file and save it without changing its contents. </p>
<p class="fragment fade-up">**mkdir** - make a directory</p>
<p class="fragment fade-up">**rmdir** - remove directory</p>
<p class="fragment fade-up">**rm** - remove file or directory</p>
<p class="fragment fade-up">**cp** - copy file or directory</p>
<p class="fragment fade-up">**mv** - move file or directory</p>

-
-
### Other commands worth knowing
<p class="fragment fade-up">**clear** - clear the terminal's display </p>
<p class="fragment fade-up">**[control]-c** - get a new terminal prompt</p>

<p class="fragment fade-up">**head** - display the first lines of a file</p>
<p class="fragment fade-up">**tail** - display the last lines of a file</p>

<p class="fragment fade-up">**more grep:**</p>
<p class="fragment fade-up">
``grep "^foo.#bar$" file.txt | grep -v "baz"``
(same  search as grep, but filter out the lines containing "baz")
</p>
<p class="fragment fade-up">
if you literally want to search for the string,
and not the regex, use fgrep (or grep -F)
``fgrep "foobar" file.txt``</p>


-
-
<img src="/reveal-slides-light/img/bunnies/cute-bunnies-tongues-3.jpg" >
