Perl 6 One Liners
=================

This file is a work in progress, converting Perl 5 one liners to Perl 6. I hope you find it interesting, maybe even useful! If you would like to contribute either bugs, new or improved regexes; issues and pull requests are welcome!

You can test the one liners on the accompanying `example.txt` file included in this repo.


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

* Matt Oates
* timotimo
* FROGGS
* Salve J Nilsen
* Alexander Moquin
* Larry Wall
* moritz

THANKS
------

Adapted from Peteris Krumins [file](http://www.catonmat.net/download/perl1line.txt). He literally wrote the [book](http://www.nostarch.com/perloneliners) on Perl one liners.

The wonderful folks on perl6 [irc](http://webchat.freenode.net/?channels=perl6&nick=):


CONTENTS
--------

1. [File Spacing](#file-spacing)
2. [Line Numbering](#line-numbering)
3. [Calculations](#calculations) (in progress)
4. [String Creation and Array Creation](#string-creation-and-array-creation)
5. [Text Conversion and Substitution](#text-conversion-and-substitution) (in progress)
6. [Selective Line Printing](#selective-line-printing) (in progress)
7. [Converting for Windows](#converting-for-windows) (in progress)


FILE SPACING
------------

Double space a file

    perl6 -pe '$_ ~= "\n"' example.txt

N-space a file (e.g. quadruple space)

    perl6 -pe '$_ ~= "\n" x 4' example.txt

Add a blank line before every line

    perl6 -pe 'say ""' example.txt

Remove all blank lines

    perl6 -ne '.print if /\S/' example.txt
    perl6 -ne '.print if .chars' example.txt

Remove all consecutive blank lines, leaving just one

    perl6 -e '$*ARGFILES.slurp.subst(/\n+/, "\n\n", :g).say' example.txt


LINE NUMBERING
--------------

Number all lines in a file

    perl6 -ne 'print ++$a ~ " $_"' example.txt
    perl6 -ne 'say $*ARGFILES.ins ~ " $_"' example.txt

Number only non-empty lines in a file

    perl6 -pe '$_ = ++$a ~ " $_" if /\S/' example.txt

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

    perl6 -ne 'say "$_ is prime" if $_.Int.is-prime'

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

Print the number PI to n decimal places

    perl -Mbignum=bpi -le 'print bpi(n)'

Print the number PI to 15 decimal places

    perl6 -e 'say π'

Print the number E to n decimal places

    perl -Mbignum=bexp -le 'print bexp(1,n+1)'

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

    tail -MDateTime::TimeZone -f logfile | perl6 -ne 'say to-timezone("GMT",DateTime.now) ~ "\t$_"'
    tail -f logfile | perl6 -ne 'say DateTime.now ~ "\t$_"'

Calculate factorial of 5

    perl6 -e 'say [*] 1..5'

Calculate greatest common divisor (GCM)

    perl -MMath::BigInt=bgcd -le 'print bgcd(@list_of_numbers)'

Calculate GCM of numbers 20 and 35 using Euclid's algorithm

    perl -le '$n = 20; $m = 35; ($m,$n) = ($n,$m%$n) while $n; print $m'

Calculate least common multiple (LCM) of numbers 35, 20 and 8

    perl -MMath::BigInt=blcm -le 'print blcm(35,20,8)'

Calculate LCM of 20 and 35 using Euclid's formula: n*m/gcd(n,m)

    perl -le '$a = $n = 20; $b = $m = 35; ($m,$n) = ($n,$m%$n) while $n; print $a*$b/$m'

Generate 10 random numbers between 5 and 15 (excluding 15)

    perl -le '$n=10; $min=5; $max=15; $, = " "; print map { int(rand($max-$min))+$min } 1..$n'

Find and print all permutations of a list

    perl -MAlgorithm::Permute -le '$l = [1,2,3,4,5]; $p = Algorithm::Permute->new($l); print @r while @r = $p->next'

Generate the power set

    perl -MList::PowerSet=powerset -le '@l = (1,2,3,4,5); for (@{powerset(@l)}) { print "@$_" }'

Convert an IP address to unsigned integer

    perl -le '$i=3; $u += ($_<<8*$i--) for "127.0.0.1" =~ /(\d+)/g; print $u'
    perl -le '$ip="127.0.0.1"; $ip =~ s/(\d+)\.?/sprintf("%02x", $1)/ge; print hex($ip)'
    perl -le 'print unpack("N", 127.0.0.1)'
    perl -MSocket -le 'print unpack("N", inet_aton("127.0.0.1"))'

Convert an unsigned integer to an IP address

    perl -MSocket -le 'print inet_ntoa(pack("N", 2130706433))'
    perl -le '$ip = 2130706433; print join ".", map { (($ip>>8*($_))&0xFF) } reverse 0..3'
    perl -le '$ip = 2130706433; $, = "."; print map { (($ip>>8*($_))&0xFF) } reverse 0..3'


STRING CREATION AND ARRAY CREATION
----------------------------------

Generate and print the alphabet

    perl6 -e 'print "a".."z"'

Generate and print all the strings from "a" to "zz"

    perl6 -e 'print "a".."zz"'

Convert a decimal number to hex using @hex lookup table

    perl -le '$num = 255; @hex = (0..9, "a".."f"); while ($num) { $s = $hex[($num%16)&15].$s; $num = int $num/16 } print $s'
    perl -le '$hex = sprintf("%x", 255); print $hex'
    perl -le '$num = "ff"; print hex $num'

Generate a random 10 a-z character string

    perl6 -e 'print roll 10, "a".."z"'
    perl6 -e 'print roll "a".."z": 10'

Generate a random 15 ASCII Character password

    perl6 -e 'print roll 15, "0".."z"'
    perl6 -e 'print roll "0".."z": 15'

Create a string of specific length

    perl6 -e 'print "a" x 50'

Generate and print an array of even numbers from 1 to 100

    perl6 -e perl6 -e '(grep { $_ % 2 == 0 }, 1..100).say'

Find the length of the string

    perl6 -e '"storm in a teacup".chars.say'

Find the number of elements in an array

    perl6 -e 'my @letters = "a".."z"; @letters.Int.say'


TEXT CONVERSION AND SUBSTITUTION
--------------------------------

ROT 13 a file

    perl6 -ne 'say .trans(("A".."Z", "a".."z") => ("N".."Z", "A".."M", "n".."z", "a".."m"))' example.txt

Base64 encode a string

    perl6 -MMIME::Base64 -ne 'print MIME::Base64.encode-str($_)' example.txt

Base64 decode a string

    perl6 -MMIME::Base64 -ne 'print MIME::Base64.decode-str($_)' base64.txt

URL-escape a string

    perl6 -MURI::Encode -le 'say uri_encode($string)'

URL-unescape a string

    perl6 -MURI::Encode -le 'say uri_decode($string)'

HTML-encode a string

    perl -MHTML::Entities -le 'print encode_entities($string)'

HTML-decode a string

    perl -MHTML::Entities -le 'print decode_entities($string)'

Convert all text to uppercase

    perl6 -ne 'say .uc' example.txt

Convert all text to lowercase

    perl6 -ne 'say .lc' example.txt

Uppercase only the first word of each line

    perl -nle 'print ucfirst lc'
    perl -nle 'print "\u\L$_"'

Invert the letter case

    perl -ple 'y/A-Za-z/a-zA-Z/'

Camel case each line

    perl -ple 's/(\w+)/\u$1/g'
    perl -ple 's/(?<!['])(\w+)/\u\1/g'

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

    perl6 -ne 'say .subst(/ut/, q/foo/, :g)' example.txt
    perl6 -pe '$_ = .subst(/ut/, q/foo/, :g)' example.txt

Find and replace all instances of "ut" with "foo" on each line that contains "lorem"

    perl6 -ne '.subst(/ut/, q/foo/).say if /Lorem/' example.txt


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

Print the last 10 lines of a file (emulate tail -10)

    perl -ne 'push @a, $_; @a = @a[@a-10..$#a]; END { print @a }'

Print only lines that contain vowels

    perl6 -ne '/<[aeiou]>/ && .print' example.txt

Print lines that contain all vowels

    perl6 -ne '/a/ && /e/ && /i/ && /o/ && /u/ && .print' example.txt

Print lines that are 80 chars or longer

    perl6 -ne '.print if .chars >= 80' example.txt
    perl6 -ne '.chars >= 80 && .print' example.txt

Print only line 2

    perl6 -ne '.print if ++$a == 2' example.txt

Print all lines except line 2

    perl6 -pe 'next if ++$a == 2' example.txt 

Print all lines 1 to 3

    perl6 -ne '.print if (1..3).any == ++$' example.txt    
    
Print all lines between two regexes (including lines that match regex)

    perl6 -ne '.print if /^Lorem/../laborum\.$/' example.txt

Print the longest line

    perl -ne '$l = $_ if length($_) > length($l); END { print $l }'

Print the shortest line

    perl -ne '$s = $_ if $. == 1; $s = $_ if length($_) < length($s); END { print $s }'

Print all lines that contain a number

    perl6 -ne '.say if /\d/' example.txt
    perl6 -ne '/\d/ && .say' example.txt
    perl6 -pe 'next if ! $_.match(/\d/)' example.txt

Find all lines that contain only a number

    perl6 -ne '.say if /^\d+$/' example.txt
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


CONVERTING FOR WINDOWS
----------------------

Running these one liners on Windows is a piece of cake once you know the rules of the road. The cardinal rule is: replace the outer single-quotes with double quotes, and use quoting operators `q//` and `qq//` (interpolated) when quoting strings inside a one liner.

Thus this one liner to prepend a blank line to every line in `example.txt`:

    perl6 -pe 'say ""' example.txt
    
Becomes:

    perl6 -pe "say q//" example.txt
