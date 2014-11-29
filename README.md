Perl 6 One Liners
=================

This book is a work in progress. I hope you find it interesting, maybe even useful! If you would like to contribute, feedback, issues and new or improved regexes are all welcome!


AUTHOR
------

David Farrell [PerlTricks.com](http://perltricks.com)


VERSION
-------

Version 0.01


LICENSE
-------

FreeBSD - see LICENSE


CONTRIBUTORS
------------

* Alexander Moquin
* FROGGS
* Helmut Wollmersdorfer
* japhb
* Larry Wall
* Matt Oates
* Moritz Lenz
* Mouq
* Salve J Nilsen
* Sam S
* timotimo


THANKS
------

Inspired by Peteris Krumins Perl 5 examples [file](http://www.catonmat.net/download/perl1line.txt). He literally wrote the [book](http://www.nostarch.com/perloneliners) on Perl 5 one liners.

The wonderful folks on #Perl6 [irc](http://webchat.freenode.net/?channels=perl6&nick=).


CONTENTS
--------

1.  [Introduction](#introduction)
2.  [Tutorial](#tutorial)
3.  [File Spacing](#file-spacing)
4.  [Line Numbering](#line-numbering)
5.  [Calculations](#calculations)
6.  [String Creation and Array Creation](#string-creation-and-array-creation)
7.  [Text Conversion and Substitution](#text-conversion-and-substitution)
8.  [Text Analysis](#text-analysis)
9.  [Selective Line Printing](#selective-line-printing)
10. [Data Transformation With Pipes](data-transformation-with-pipes) (in progress)
11. [WWW](#www) (in progress)
12. [Converting for Windows](#converting-for-windows)


INTRODUCTION
------------

One thing that sets Perl apart from other languages is the ability to write small programs in a single line of code, known as a "one liner". It's often faster to type a program directly into the terminal than to write a throwaway script. And one liners are powerful too; they're fully fledged programs which can load external libraries but also integrate into the terminal. You can pipe data in or out of a one liner.

Like Perl 5, Perl 6 supports one liners. And just like Perl 6 cleaned up Perl 5's warts elsewhere, the one liner syntax is also better. It's cleaner with fewer special variables and options to memorize. This book provides many useful examples of Perl 6 one liners that can do everything from finding duplicate lines in a file to running a web server. Although Perl 6 has fewer special variables, because of its advanced object oriented syntax most of the one liners are shorter in Perl 6 than their Perl 5 equivalent.

This book can be read in a number of ways. If you're new to one liners, start with the (tutorial)[#tutorial]. It walks you through the core concepts of a one liner; don't worry - it's really very simple once you get the hang of it. If you're familiar with Perl, Bash or Sed/Awk, you can probably get stuck in to the examples right away. Feel free to skim and scan the material for whatever piques your interest. If you don't understand some code, try it out in the terminal! Included in this repo is the ubiquitous `example.txt` file which is used in many of the one liners.

Programming with one liners is just one paradigm that Perl 6 excels in. There's a beauty in the brevity of this code, but whilst you're learning a productive skill, remember that you're also learning the ropes of a powerful new programming language. Check out the [perl6.org](http://perl6.org) website for the official documentation.


TUTORIAL
--------

To get started with one liners, all you really need to understand is the `-e` option. This tells Perl to execute what follows as a program. For example:

    perl6 -e 'say "Hello, World!"'

Let's step through this code. `perl6` invokes the Perl 6 program, `-e` tells Perl 6 to execute and `'say "Hello, World!"'` is the program. Every program must be surrounded in single quotes (except on Windows, see [Converting for Windows](#converting-for-windows)). To run the one-liner, just type it into the terminal:

    > perl6 -e 'say "Hello, World!"'
    Hello, World!

If you want to load a file, just add the path to the file after the program code:

    perl6 -e 'for (lines) { say $_ }' /path/to/file.txt

This program prints every line in `/path/to/file.txt`. You may know that `$_` is the default variable, which in this case is the current line being looped through. `lines` is a list that is automatically created for you whenever you pass a filepath to a one-liner. Now let's re-write that one liner, step-by-step. These are all equivalent:

    perl6 -e 'for (lines) { say $_ }' /path/to/file.txt
    perl6 -e 'for (lines) { $_.say }' /path/to/file.txt
    perl6 -e 'for (lines) { .say }' /path/to/file.txt
    perl6 -e '.say for (lines)' /path/to/file.txt
    perl6 -e '.say for lines' /path/to/file.txt

Just like `$_` is the default variable, methods called on the default variable can omit the variable referece. They become default methods. So `$_.say` becomes `.say`. This brevity pays off with one liners - it's less typing!

The `-n` option changes the behavior of the program: it executes the code once for every line of the file. So uppercase and print every line of `/path/to/file.txt` you can type:

    perl6 -ne '.uc.say' /path/to/file.txt

The `-p` option is just like `-n` except that it will automatically print `$_`. So another way we could uppercase a file would be:

    perl6 -pe '$_ = .uc' /path/to/file.txt

Or two shorter versions that do the same thing:

    perl6 -pe '.=uc' /path/to/file.txt
    perl6 -pe .=uc /path/to/file.txt

In the second example we were able to completely remove the surrounding single quotes. This is a rare scenario, but in the event your one liner has no spaces and no sigils or quotes in it, you can usually remove the outer quotes.

The `-n` and `-p` options are really useful. There are lots of example one-liners that use them in this book.

The final thing you should know is how to load a module. This is really powerful as you can extend Perl 6's capabilities by importing external libraries. The `-M` switch stands for load module:

    perl6 -M URI::Encode -e 'say encode_uri("example.com/10 ways to crush it with Perl 6")'

This: `-M URI::Encode` loads the URI::Encode module, which exports the `encode_uri` subroutine. You can use `-M` more than once if you want to load more than one module:

    perl6 -M URI::Encode -M URI e- '<your code here>'

What if you have a local module, that is not installed yet? Easy, just pass use the `-I` switch to include the directory:

    perl6 -I lib -M URI::Encode -e '<your code here>'

Now Perl 6 will search for `URI::Encode` in `lib` as well as the standard install locations.

To get a list of Perl 6 command line switches, use the `-h` option for help:

    perl6 -h

This prints a nice summary of the available options.


FILE SPACING
------------

Double space a file

    perl6 -pe '$_ ~= "\n"' example.txt

N-space a file (e.g. quadruple space)

    perl6 -pe '$_ ~= "\n" x 4' example.txt

Add a blank line before every line

    perl6 -pe 'say ""' example.txt

Remove all blank lines

    perl6 -ne '.say if /\S/' example.txt
    perl6 -ne '.say if .chars' example.txt

Remove all consecutive blank lines, leaving just one

    perl6 -e '$*ARGFILES.slurp.subst(/\n+/, "\n\n", :g).say' example.txt


LINE NUMBERING
--------------

Number all lines in a file

    perl6 -ne 'say "{++$} $_"' example.txt
    perl6 -ne 'say $*ARGFILES.ins ~ " $_"' example.txt

Number only non-empty lines in a file

    perl6 -pe '$_ = "{++$} $_" if /\S/' example.txt

Number all lines but print line numbers only for non-empty lines

    perl6 -pe '$_ = $*ARGFILES.ins ~ " $_" if /\S/' example.txt

Print the total number of lines in a file (emulate wc -l)

    perl6 -e 'say lines.elems' example.txt
    perl6 -e 'say lines.Int' example.txt
    perl6 -e 'lines.Int.say' example.txt

Print the number of non-empty lines in a file

    perl6 -e 'lines.grep(/\S/).elems.say' example.txt

Print the number of empty lines in a file

    perl6 -e 'lines.grep(/\s/).elems.say' example.txt


CALCULATIONS
------------

Check if a number is a prime

    perl6 -e 'say "7 is prime" if 7.Int.is-prime'

Print the sum of all the fields on a line

    perl6 -ne 'say [+] .split("\t")'

Print the sum of all the fields on all lines

    perl6 -e 'say [+] lines.split("\t")'

Shuffle all fields on a line

    perl6 -ne '.split("\t").pick(*).join("\t").say'

Find the minimum element on a line

    perl6 -ne '.split("\t").min.say'

Find the minimum element over all the lines

    perl6 -e 'lines.split("\t").min.say'

Find the maximum element on a line

    perl6 -ne '.split("\t").max.say'

Find the maximum element over all the lines

    perl6 -e 'lines.split("\t").max.say'

Replace each field with its absolute value

    perl6 -ne '.split("\t").map(*.abs).join("\t").say'

Find the total number of letters on each line

    perl6 -ne '.chars.say' example.txt

Find the total number of words on each line

    perl6 -ne '.words.elems.say' example.txt

Find the total number of elements on each line, split on a comma

    perl6 -ne '.split(",").elems.say' example.txt

Find the total number of fields (words) on all lines

    perl6 -e 'say lines.split("\t").elems' #fields
    perl6 -e 'say lines.words.elems' example.txt #words

Print the total number of fields that match a pattern

    perl6 -e 'say lines.split("\t").comb(/pattern/).elems' #fields
    perl6 -e 'say lines.words.comb(/pattern/).elems' #words

Print the total number of lines that match a pattern

    perl6 -e 'say lines.grep(/in/).elems'

Print the number PI to n decimal places (e.g. 10)

    perl6 -e 'say pi.fmt("%.10f");

Print the number PI to 15 decimal places

    perl6 -e 'say π'

Print the number E to n decimal places (e.g. 10)

    perl6 -e 'say e.fmt("%.10f");

Print the number E to 15 decimal places

    perl6 -e 'say e'

Print UNIX time (seconds since Jan 1, 1970, 00:00:00 UTC)

    perl6 -e 'say time'

Print GMT (Greenwich Mean Time) and local computer time

    perl6 -MDateTime::TimeZone -e 'say to-timezone("GMT",DateTime.now)'
    perl6 -e 'say DateTime.now'

Print local computer time in H:M:S format

    perl6 -e 'say DateTime.now.map({$_.hour, $_.minute, $_.second.round}).join(":")'

Print yesterday's date

    perl6 -e 'say DateTime.now.earlier(:1day)'

Print date 14 months, 9 days and 7 seconds ago

    perl6 -e 'say DateTime.now.earlier(:14months).earlier(:9days).earlier(:7seconds)'

Prepend timestamps to stdout (GMT, localtime)

    tail -f logfile | perl6 -MDateTime::TimeZone -ne 'say to-timezone("GMT",DateTime.now) ~ "\t$_"'
    tail -f logfile | perl6 -ne 'say DateTime.now ~ "\t$_"'

Calculate factorial of 5

    perl6 -e 'say [*] 1..5'

Calculate greatest common divisor

    perl6 -e 'say [gcd] @list_of_numbers'

Calculate GCM of numbers 20 and 35 using Euclid's algorithm

    perl6 -e 'say (20, 35, *%* ... 0)[*-2]'

Calculate least common multiple (LCM) of 20 and 35

    perl6 -e 'say 20 lcm 35'

Calculate LCM of 20 and 35 using Euclid's algorithm: `n*m/gcd(n,m)`

    perl6 -e 'say 20 * 35 / (20 gcd 35)'

Generate 10 random numbers between 5 and 15 (excluding 15)

    perl6 -e '.say for (5..^15).roll(10)'

Find and print all permutations of a list

    perl6 -e 'say .join for [1..5].permutations'

Generate the power set

    perl6 -e '.say for <1 2 3>.combinations'

Convert an IP address to unsigned integer

    perl6 -e 'say :256["127.0.0.1".comb(/\d+/)]'
    perl6 -e 'say +":256[{q/127.0.0.1/.subst(:g,/\./,q/,/)}]"'
    perl6 -e 'say Buf.new(+«"127.0.0.1".split(".")).unpack("N")'

Convert an unsigned integer to an IP address

    perl6 -e 'say join ".", @(pack "N", 2130706433)'
    perl6 -e 'say join ".", map { ((2130706433+>(8*$_))+&0xFF) }, (3...0)'

STRING CREATION AND ARRAY CREATION
----------------------------------

Generate and print the alphabet

    perl6 -e '.say for "a".."z"'

Generate and print all the strings from "a" to "zz"

    perl6 -e '.say for "a".."zz"'

Convert a integer to hex

    perl6 -e 'say 255.base(16)'
    perl6 -e 'say sprintf("%x", 255)'

Print an int to hex translation table

    perl6 -e 'say sprintf("%3i => %2x", $_, $_) for 0..255'

Percent encode an integer

    perl6 -e 'say sprintf("%%%x", 255)'

Generate a random 10 a-z character string

    perl6 -e 'print roll 10, "a".."z"'
    perl6 -e 'print roll "a".."z": 10'

Generate a random 15 ASCII Character password

    perl6 -e 'print roll 15, "0".."z"'
    perl6 -e 'print roll "0".."z": 15'

Create a string of specific length

    perl6 -e 'print "a" x 50'

Generate and print an array of even numbers from 1 to 100

    perl6 -e '(1..100).grep(* %% 2).say'

Find the length of the string

    perl6 -e '"storm in a teacup".chars.say'

Find the number of elements in an array

    perl6 -e 'my @letters = "a".."z"; @letters.Int.say'


TEXT CONVERSION AND SUBSTITUTION
--------------------------------

ROT 13 a file

    perl6 -pe 'tr/A..Za..z/N..ZA..Mn..za..m/' example.txt

Base64 encode a string

    perl6 -MMIME::Base64 -ne 'print MIME::Base64.encode-str($_)' example.txt

Base64 decode a string

    perl6 -MMIME::Base64 -ne 'print MIME::Base64.decode-str($_)' base64.txt

URL-escape a string

    perl6 -MURI::Encode -le 'say uri_encode($string)'

URL-unescape a string

    perl6 -MURI::Encode -le 'say uri_decode($string)'

HTML-encode a string

    perl6 -MHTML::Entity -e 'print encode-entities($string)'

HTML-decode a string

    perl6 -MHTML::Entity -e 'print decode-entities($string)'

Convert all text to uppercase

    perl6 -pe '.=uc' example.txt
    perl6 -ne 'say .uc' example.txt

Convert all text to lowercase

    perl6 -pe '.=lc' example.txt
    perl6 -ne 'say .lc' example.txt

Uppercase only the first word of each line

    perl6 -ne 'say s/(\w+){}/{$0.uc}/' example.txt

Invert the letter case

    perl6 -pe 'tr/a..zA..Z/A..Za..z/' example.txt
    perl6 -ne 'say tr/a..zA..Z/A..Za..z/.after' example.txt

Camel case each line

    perl6 -ne 'say .wordcase' example.txt

Strip leading whitespace (spaces, tabs) from the beginning of each line

    perl6 -ne 'say .trim-leading' example.txt

Strip trailing whitespace (space, tabs) from the end of each line

    perl6 -ne 'say .trim-trailing' example.txt

Strip whitespace from the beginning and end of each line

    perl6 -ne 'say .trim' example.txt

Convert UNIX newlines to DOS/Windows newlines

    perl6 -ne 'print .subst(/\n/, "\r\n")' example.txt

Convert DOS/Windows newlines to UNIX newlines

    perl6 -ne 'print .subst(/\r\n/, "\n")' example.txt

Find and replace all instances of "ut" with "foo" on each line

    perl6 -pe 's:g/ut/foo/' example.txt

Find and replace all instances of "ut" with "foo" on each line that contains "lorem"

    perl6 -pe 's:g/ut/foo/ if /Lorem/' example.txt

Convert a file to JSON

    perl6 -M JSON::Tiny -e 'say to-json(lines)' example.txt

Pick 5 random words from each line of a file

    perl6 -ne 'say .words.pick(5)' example.txt


TEXT ANALYSIS
-------------

Print n-grams of a string

    perl6 -e 'my $n=2; say "banana".comb.rotor($n,$n-1).map({[~] @$_})'

Print unique n-grams

    perl6 -e 'my $n=2; say "banana".comb.rotor($n,$n-1).map({[~] @$_}).Set.sort'

Print occurrence counts of n-grams

    perl6 -e 'my $n=2; say "banana".comb.rotor($n,$n-1).map({[~] @$_}).Bag.sort.join("\n")'

Print occurence counts of words (1-grams)

    perl6 -e 'say lines[0].words.map({[~] @$_}).Bag.sort.join("\n")' example.txt

Print Dice similarity coefficient based on sets of 1-grams

    perl6 -e 'my $a="banana".comb;my $b="anna".comb;say ($a (&) $b)/($a.Set + $b.Set)'

Print Jaccard similarity coefficient based on 1-grams

    perl6 -e 'my $a="banana".comb;my $b="anna".comb;say ($a (&) $b) / ($a (|) $b)'

Print overlap coefficient based on 1-grams

    perl6 -e 'my $a="banana".comb;my $b="anna".comb;say ($a (&) $b)/($a.Set.elems,$b.Set.elems).min'

Print cosine similarity based on 1-grams

    perl6 -e 'my $a="banana".comb;my $b="anna".comb;say ($a (&) $b)/($a.Set.elems.sqrt*$b.Set.elems.sqrt)'

Build an index of characters within a string and print it

    perl6 -e 'say {}.push: %("banana".comb.pairs).invert'
 
Build an index of words within a line and print it

    perl6 -e '({}.push: %(lines[0].words.pairs).invert).sort.join("\n").say' example.txt


SELECTIVE LINE PRINTING
-----------------------

Print the first line of a file (emulate head -1)

    perl6 -ne '.say;exit' example.txt
    perl6 -e 'lines[0].say' example.txt
    perl6 -e 'lines.shift.say' example.txt

Print the first 10 lines of a file (emulate head -10)

    perl6 -pe 'exit if ++$ > 10' example.txt
    perl6 -ne '.say if ++$ < 11' example.txt

Print the last line of a file (emulate tail -1)

    perl6 -e 'lines.pop.say' example.txt

Print the last 5 lines of a file (emulate tail -5)

    perl6 -e '.say for lines[^5]' example.txt

Print only lines that contain vowels

    perl6 -ne '/<[aeiou]>/ && .print' example.txt

Print lines that contain all vowels

    perl6 -ne '.say if .comb (>=) <a e i o u>' example.txt
    perl6 -ne '.say if .comb ⊇ <a e i o u>' example.txt

Print lines that are 80 chars or longer

    perl6 -ne '.print if .chars >= 80' example.txt
    perl6 -ne '.chars >= 80 && .print' example.txt

Print only line 2

    perl6 -ne '.print if ++$ == 2' example.txt

Print all lines except line 2

    perl6 -pe 'next if ++$ == 2' example.txt

Print all lines 1 to 3

    perl6 -ne '.print if (1..3).any == ++$' example.txt

Print all lines between two regexes (including lines that match regex)

    perl6 -ne '.print if /^Lorem/../laborum\.$/' example.txt

Print the length of the longest line

    perl6 -e 'say lines.max.chars' example.txt
    perl6 -ne 'state $l=0; $l = .chars if .chars > $l;END { $l.say }' example.txt

Print the longest line

    perl6 -e 'say lines.max' example.txt
    perl6 -e 'my $l=""; for (lines) {$l = $_ if .chars > $l.chars};END { $l.say }' example.txt

Print all lines that contain a number

    perl6 -ne '.say if /\d/' example.txt
    perl6 -e '.say for lines.grep(/\d/)' example.txt
    perl6 -ne '/\d/ && .say' example.txt
    perl6 -pe 'next if ! $_.match(/\d/)' example.txt

Find all lines that contain only a number

    perl6 -ne '.say if /^\d+$/' example.txt
    perl6 -e '.say for lines.grep(/^\d+$/)' example.txt
    perl6 -ne '/^\d+$/ && .say' example.txt
    perl6 -pe 'next if ! $_.match(/^\d+$/)' example.txt

Print every odd line

    perl6 -ne '.say if ++$ % 2' example.txt

Print every even line

    perl6 -ne '.say if ! (++$ % 2)' example.txt

Print all lines that repeat

    perl6 -ne 'state %l;.say if ++%l{$_}==2' example.txt

Print unique lines

    perl6 -ne 'state %l;.say if ++%l{$_}==1' example.txt

Print the first field (word) of every line (emulate cut -f 1 -d ' ')

    perl6 -ne '.words[0].say' example.txt


DATA TRANSFORMATION WITH PIPES
------------------------------

Perl 6 progams integrate straight into the command line. You can pipe data in-to and out-of a one liner by using the pipe `|` character. For piping data in, Perl 6 automatically sets STDIN to `$*IN`. Just like with files, data piped in can be looped through using `-n` and is also available in `lines`. To pipe data out of a one liner just use `print` or `say`.

JSON-encode a list of all files in the current directory

    ls | perl6 -M JSON::Tiny -e 'say to-json(lines)'

Print a random sample of approx 5% of lines in a file

    perl6 -ne '.say if 1.rand <= 0.05' /usr/share/dict/words


WWW
---

Download a webpage

    perl6 -M HTTP::UserAgent -e 'say HTTP::UserAgent.new.get("google.com").content

Download a webpage and strip out the HTML

    wget -O - "http://perl6.org" | perl6 -ne 's:g/\<.+?\>//.say'

Download a webpage, strip out and decode the HTML

    wget -O - "http://perl6.org" | perl6 -MHTML::Strip -ne 'strip_html($_).say'

Launch a simple web server

    perl6 -M HTTP::Server::Simple -e 'HTTP::Server::Simple.new.run'


CONVERTING FOR WINDOWS
----------------------

Running one liners on Windows is a piece of cake once you know the rules of the road. One liners work on both cmd.exe and PowerShell. The cardinal rule is: replace the outer single-quotes with double quotes and use the interpolated quoting operator `qq//` for quoting strings inside a one liner. For non-interpolated quoting, you can use single-quotes. Let's look at some examples.

Here's a simple one liner to print the time:

    perl6 -e 'say DateTime.now'

To run on Windows, we just replace the single quotes with double quotes:

    perl6 -e "say DateTime.now"

This one liner appends a newline to every line in a file, using an interpolated string:

    perl6 -pe '$_ ~= "\n"' example.txt

On Windows this should be written as:

    perl6 -pe "$_ ~= qq/\n/" example.txt

In this case we want to interpolate `\n` as a newline and not literally add a backslash and an "n" to the line, so we have to use `qq`. But you can usually use single-quotes within a one liner so:

    perl6 -e 'say "Hello, World!"'

Can be written as:

    perl6 -e "say 'hello, World!'"

Simple output redirection works like it does on Unix-based systems. This one liner prints an ASCII character index table to a file using `>`:

    perl6 -e "say .chr ~ ' ' ~ $_ for 0..255" > ascii_codes.txt

When using `>` if the file doesn't exist, it will be created. If the file does exist, it will be overwritten. If you'd rather append to a file, use `>>` instead.

