---
layout: post
title: "Formatting output of hexdump"
categories: tools
---

Usually, I just run `hexdump -C` command to view a binary file in hex format along with the corresponding ASCII characters on the right, where printable. This is the so-called "canonical" format. `hexdump` provides a few more built-in formats which cover the general use cases.
```
002bb900  65 74 61 00 00 00 00 00  00 00 21 68 64 6c 72 00  |eta.......!hdlr.|
002bb910  00 00 00 00 00 00 00 6d  64 69 72 61 70 70 6c 00  |.......mdirappl.|
002bb920  00 00 00 00 00 00 00 00  00 00 00 2d 69 6c 73 74  |...........-ilst|
002bb930  00 00 00 25 a9 74 6f 6f  00 00 00 1d 64 61 74 61  |...%.too....data|
002bb940  00 00 00 01 00 00 00 00  4c 61 76 66 35 38 2e 31  |........Lavf58.1|
002bb950  32 2e 31 30 30                                    |2.100|
002bb955
```
But, have you ever wondered if we can specify our own formatting? Sometimes, we may want to not output the offset so that we can diff two hexdumps more effectively, for instance. The good news is that `hexdump` does support advanced formatting. However, it may take some time to comprehend.

To understanding how formatting works, here is an example of custom formatting to achieve the "canonical" output with an addition that the offset is printed in <span style="color: cyan">color</span>!

# Example 1: Mimic `hexdump -C`
{% highlight bash %}
hexdump --color \
 -e '"%07_ax_L[cyan]  " 8/1 "%02x " "  " 8/1 "%02x "' \
 -e '"  |" 16/1 "%_p" "|\n"' \
 file.bin
{% endhighlight %}

Here, we have passed two format strings using a `-e` switch for each. A format string consists of one or more format units. In the first string, there are four units:
```
"%07_ax_L[cyan]  "	# print offset, in color
" 8/1 "%02x "		# print 8 bytes as hex separated by a space 
"  "			# separate the two 8-byte columns by some space
" 8/1 "%02x "		# repeat the second format unit
```
For convenience, the format strings can be put in a file which can be passed to `hexdump` with option `-f` instead of passing them inline with `-e`.

If the final offset needs to be printed at the end of the output like `hexdump -C` does, then an additional format string needs to be added (and indicated in the `hexdump` man page):
```
 "%07_Ax_L[cyan]  "
```

The final format file would like:
```
# file: canonical
"%07_Ax_L[cyan]  "
"%07_ax_L[cyan]  " 8/1 "%02x " "  " 8/1 "%02x "
"  |" 16/1 "%_p" "|\n"
```

Then, the invocation would be:
{% highlight bash %}
hexdump -L -f canonical file.bin
{% endhighlight %}

*Note:* `--color` and `-L` options are equivalent.

Now, coming to the original case of omitting the offset from the output, removing  the `"%07_ax_L[cyan] "` format string will do the trick.
