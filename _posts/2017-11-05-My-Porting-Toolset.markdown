---
layout:     post
title:      "My porting toolset"
subtitle:   "Feel the power of god"
date:       2017-11-05 15:00:00
author:     "z"
header-img: "img/2017-11-05-porting-toolset.jpg"
---

This is a short blogpost about the tools and scripts what prooved to be useful during my porting work. Some could be handy for you too.

---------

## The Silver Searcher

Have you ever had to grep hundreds of Mb source code? Then you know how painfully slow it is. But the good news: it doesn't have to be slow. I'd like to introduce "ag" to you, the command line program known as "The Silver Searcher" too.
"ag" using optimized code to search for the required string in big bunch of code, it inteligently excludes irrelevant files and it can use regex too.
Because its speed many scripts in my toolset using directly or indirectly "ag".

---------

## ZSH / Fish

Hovewer i'm okay with bash (at least since i studied a book about it), there is some friendlier shells available for Haiku with great extensibility. As almost every shell script start with sh or bash shebang and as only bash provides "sh", they can coxist and using an alternative shell means you get only the pros, never the cons. You have to configure them for your workflow, but it is a small price.

For example, while you can `CTRL-R` in bash to recall a command from bash history, in fish you can just start typing and it will try to complete your command. If there are more possible matches you can select the correct one with `UP` and `DOWN` arrow keys.  With some modules you can replicate this functionality in zsh too.

---------

### "ag" script:

This is a really simple wrapper to ag, predefines some optional switches, so it will highlight the matches, cuts the long lines at 70 chars and prints the line numbers too.

The cool thing is, i don't need to supply any PATH or filename, it will automatically and recursively search in all the files and folders in the current work folder.

	function ag {
		command ag -i -Q -t -W 70 --color-match 31\;31 --color-line-number 2\;33 --color-path 2\;32 $argv;
	}

Example:

`# ag some_stuff`

------

### "inrecipe" script:

Working with the HaikuPorts tree is not easy ride. We try to follow the Gentoo Portage and the Arch AUR logical folder structure, but some recipes still resides in unreasonable places. To find them I have to "ag" trough the whole tree, but i let ag analyze only the files with "recipe" extension and it won't go recursively in "work" folders to spare time.

	function inrecipe {
		/bin/ag $argv /Ports/haikuports -G recipe --ignore "work*"
		}

If i update a port and want to know which other ports depending on it, i can easily "grep" the recipes.
So if i work on "libSDL" i can get the depending ports like:

`# inrecipe libSDL`

-----------

### "searchrecipe" script:

Before you trying to port something, it is always a good idea to check if it is already ported by somebody else. As i told, some recipes resides still in unreasonable place, so it is always good idea to check the whole HaikuPorts tree.

	function searchrecipe {
		query name="*$argv*.recipe" -v /Ports/haikuports
	}

So if i want to know if "libSomething" already available or not, i can just type:

`# searchrecipe libSomething`

------------

### "inpatch" script:

During my porting work i met recurring problems, like "DIRENT" not declared. As i don't like to reinvent warm water, i usally check the patches in the HaikuPorts tree, and hoping somebody already did the dirty job and if so, i can just copy the working fix.

	function inpatch {
		/bin/ag $argv /Ports/haikuports -G patch --ignore "work*"
	}

So, did you got problems with undeclared "FD_SET"? Just "grep" the patches!

`# inpatch FD_SET`

--------------

### "indev" script:

Haiku have some functions declared in unorthodox header files, to find out where are they, you can also... yes... grep the header folder! :)

	function indev {
		ag $argv `finddir B_SYSTEM_HEADERS_DIRECTORY`
	}

See? nothing difficult.

---------

### "dependencies" script:

Have you got an executable and wish to see the runtime dependencies? "objdump" can give you a detailed answer, but it is too long, so.. yeah, just "grep" it!

	function dependencies {
		objdump -x $argv | ag NEEDED
	}

`# dependencies quake-SDL`

----------

### "cleanup" script:

Travis will complain if you left whitespace characters at the end of the lines in your recipe, so let's purge them with sed!

	function cleanup {
		sed --in-place 's/[[:space:]]\+$//' $argv
	}

`# cleanup my_recipe.recipe`

------------

### "crnb" script:

If i want to try blindly build something, without prior experience, just to see, what will happen, i can user my precious "crnb" script.
It contains the most important LD and CFLAGS. Plenty times it is just enough to get  stuff compiled. And i can still pass some other --switches to it.

	function crnb {
		./configure LDFLAGS="-lnetwork -lbsd" CFLAGS="-D_BSD_SOURCE" $argv
	}

`# crnb --extra-switches`

-----------

### "hp_clean" script:

After some heavy porting session you will notice how fast your free space decreases. This script deletes all the working folders and all the downloadad tarballs.

	function hp_clean {
		find /Ports/haikuports -type d -name "work-*" -exec rm -rf {} \
		find /Ports/haikuports -type d -name "download" -exec rm -rf {} \
	}

Nice! No example required i supose.

---------------

### "share" script:

Have you got a text-file, a config log or a makefile output what you want to share? Pastebin? Eh, you can do better in the terminal. This script uploads your text file to 0x0.st and provides you a direct link. No ads, no bullshit.

	function share {
		curl -F"file=@$argv" https://0x0.st
	}

It can do images and other files too, but stores them only for some days.

`# share config.log`

----------------

### "chunkio" script:

PulkoMandy's ISP don't like 0x0, so i had to find an another paste-bin like  provider. This works for him too, but to upload you have to register first.

	function chunkio {
		curl -u user_name:password -sT $argv chunk.io
	}

`# chunkio screenshot.png`

----------------

### "tb" script:

Do you want to upload the output of a command, like "make"? You can copy it from the Terminal window, paste in a file and then just upload with the earlier commands, or you can just pipe it to tb!

	function tb {
		nc termbin.com 9999 $argv;
	}

`# make | tb`

------------------

### "hp" script:

The well known hp script with predefined jobs, and disabled source packages, i don't need them.

	function hp {
		haikuporter -S -j4 --no-source-packages $argv;
	}

----------------

That was all my scripts. As you can see, i better try to reuse the available solutions instead of reinventing them. The scripts are made for "zsh" shell, but i have them for fish too, and it isn't impossible to implement them bash-friendly. 
