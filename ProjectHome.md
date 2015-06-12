![http://img12.imageshack.us/img12/4361/markdownsharplogo.png](http://img12.imageshack.us/img12/4361/markdownsharplogo.png)

Open source C# implementation of [Markdown](http://daringfireball.net/projects/markdown/) processor, as featured on [Stack Overflow](http://stackoverflow.com).

This port is based heavily on the original [Perl 1.0.1](http://code.google.com/p/markdownsharp/source/browse/MarkdownSharpTests/source/perl/Markdown.101.pl) and [Perl 1.0.2b8](http://code.google.com/p/markdownsharp/source/browse/MarkdownSharpTests/source/perl/Markdown.102b8.pl) implementations of Markdown, with bits and pieces of the apparently much better maintained [PHP Markdown](http://michelf.com/projects/php-markdown/) folded into it. There are a few Stack Overflow specific modifications (which are all configurable, and all off by default). I'd like to ensure that this version stays within shouting distance of the Markdown "specification", such as it is...

# Please contribute! #

You'll need to know:

  * C#
  * Regular Expressions (I mean _really freaking know them_ -- the original Markdown implementation was in Perl. Consider yourselves warned.)
  * Markdown

**If you'd like to contribute [file issues](http://code.google.com/p/markdownsharp/issues/list) and attach `*.patch` files of proposed fixes**. Beyond that, we'll see, but I do plan to open it up over time to trusted contributors.

Please note that as of January 8, 2011 MarkdownSharp uses Mercurial for version control; the old **Subversion repository will not be updated anymore**.
## Tests ##

I pulled in the [MDTest 1.1 test suite](http://code.google.com/p/markdownsharp/source/browse/#hg%2FMarkdownSharpTests%2Ftestfiles%2Fmdtest-1.1), along with some very rudimentary unit tests of my own. Results of `Test(@"mdtest-1.1")`:

```
MarkdownSharp v1.12 test run on \mdtest-1.1

001 Amps_and_angle_encoding                                OK
002 Auto_links                                             OK
003 Backslash_escapes                                      OK^
004 Blockquotes_with_code_blocks                           OK
005 Code_Blocks                                            OK
006 Code_Spans                                             OK
007 Hard_wrapped_paragraphs_with_list_like_lines           OK
008 Horizontal_rules                                       OK
009 Images                                                 OK
010 Inline_HTML_Advanced                                   OK
011 Inline_HTML_comments                                   OK
012 Inline_HTML_Simple                                     OK
013 Links_inline_style                                     OK
014 Links_reference_style                                  OK
015 Links_shortcut_references                              OK
016 Literal_quotes_in_titles                               OK
017 Markdown_Documentation_Basics                          OK
018 Markdown_Documentation_Syntax                          OK
019 Nested_blockquotes                                     OK
020 Ordered_and_unordered_lists                            OK^
021 Strong_and_em_together                                 OK
022 Tabs                                                   OK
023 Tidyness                                               OK^

Tests        : 23
OK           : 23 (^ 3 whitespace differences)
Mismatch     : 0
```

The [original C# port](http://aspnetresources.com/blog/markdown_1_0_1_7.aspx) is included as [MarkdownOld](http://code.google.com/p/markdownsharp/source/browse/MarkdownSharp/MarkdownOld.cs) for comparison. There were some port-specific bugs, but most of the bugs were inherited from the original circa-2004 `Markdown.pl` (v1.0.1) the library was based on. I've been pulling across some fixes from a newer circa-2007 `Markdown.pl` (v.1.0.2b8). I have included both of these in the [\source\perl](http://code.google.com/p/markdownsharp/source/browse/#hg%2FMarkdownSharpTests%2Fsource%2Fperl) folder so it's easier to compare them to their C# counterparts.

To see what's wrong, diff the `*.html` and `*.actual.html` files generated from `Test(@"mdtest-1.1")`. (Note that these files are only generated if the actual output is different from the expected output.) Feel free to open issues anything in particular that you feel should be worked on first, based on [existing bug reports](http://meta.stackoverflow.com/questions/tagged/markdown+bug).

## Benchmarks ##

This version is many times faster than the original. The built-in `Benchmark()` routine is provided for benchmarking.

**Markdown.NET**

<pre>
input string length: 475<br>
4000 iterations in 18843 ms   (4.71075 ms per iteration)<br>
input string length: 2356<br>
1000 iterations in 8992 ms    (8.992 ms per iteration)<br>
input string length: 27737<br>
100 iterations in 15016 ms    (150.16 ms per iteration)<br>
input string length: 11075<br>
1 iteration in 55 ms<br>
input string length: 88607<br>
1 iteration in 672 ms<br>
input string length: 354431<br>
1 iteration in 6375 ms<br>
</pre>

**MarkdownSharp** v1.12

<pre>
input string length: 475<br>
4000 iterations in 1778 ms (0.4445 ms per iteration)<br>
input string length: 2356<br>
1000 iterations in 1791 ms (1.791 ms per iteration)<br>
input string length: 27737<br>
100 iterations in 1932 ms (19.32 ms per iteration)<br>
input string length: 11075<br>
1 iteration in 10 ms<br>
input string length: 88607<br>
1 iteration in 74 ms<br>
input string length: 354431<br>
1 iteration in 306 ms<br>
</pre>