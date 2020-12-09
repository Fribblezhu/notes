# markdown notes
## headers
---
# this is an h1
## this is an h2
### this is an h3
#### this is an h4
##### this is an h5
---

## block quotes
---
Markdown allow you to be lazy and only put the > before the __first line__ or a __hard-wrapped__ paragraph.

>  it looks best if you hard wrap the text and put an > before every line;
> > block quotes can be __nested__
> > > block quotes can be nested
> ### block quotes can contain other markdown elements
---
## lists
---
Markdown supports __ordered__ (numbered) and __unordered__(bulleted) list
> * red
- green
+ red
1. bird
1. mchale
4. parish
2. it's important to note that the actual numbers you use to mark the list have no effect on the HTML output markdown produces
8. a list item with a block quote
>> this is a block quote
>> inside a list item
9. a list item with a code block：
    <code>code goes here</code>
>10. 1986\. what a great season
---
## code blocks
---
<p> To produce a code block in markdown. simply indent every line of the block by at least 4 spaces or 1 tab.</p>

> <p>this is a normal paragraph</p>
> <pre>empty container</pre>
> <pre><code>This is a code block.</code></pre>

---
## horizontal rules ##
---
You can produce a horizontal rule as follows:
> - use hr tag
 <hr />
> - use ---
> ---
> - use ***
> ***
---

### links
---
Markdown supports two style of links: __inline__ and __reference__
>
> this is [an example](http://www.google.com) inline link.
>
> if you're referring to a local resource on the same server, you can use relative paths:
> See my [About](/home/zhu.wenjian/.m2/setting.xml)
___
## emphasis
---
> markdown treats asterisks(*) and underscores(_) as indicators of emphasis
>
> *markdown treats asterisks(*) and underscores(_) as indicators of emphasis*
>
>  __markdown treats asterisks(*) and underscores(_) as indicators of emphasis__
>
> ***markdown treats asterisks(*) and underscores(_) as indicators of emphasis***
---
## code
---
to indicate a span of code ,wrap it with backtick quotes (`)
> Use the `printf()` function or Use the `` `printf()` ``
---
## images
---
>![Alt text](/home/zhu.wenjian/Downloads/ecf708685436b78804a5093d8ef732d.jpg "this is image name")
---
## automatic links
---
Markdown supports a shortcut style for creating automatic link: simply surround the URL or email  address with angle brackets.

<http://www.automatic.com>

---
## backslash escapes
---
Markdown allows you to use backslash escapes to generate literal characters which would otherwise have special meaning in meaning in markdown's formatting syntax.for example:

>\*literal asterisks\*

markdown provides backslash escapes for the following characters:
- \\  backslash
- \`  back tick
- \*  asterisk
- \_  underscore
- {}  curly braces
- []  square brackets
- ()  parentheses
- \#  hash mark
- \+  plus sign 
- \-  minus sign
- \.  dot
- \!  exclamation mark
---
