# Markdown

Botcopy components support most unicode characters including emoji 📱. We also support the following markdown formatting in responses:

## Headers

Add headers to your bot's responses.

```
# H1
## H2
### H3
#### H4
##### H5
###### H6

Alternatively, for H1 and H2, an underline-ish style:

Alt-H1
======

Alt-H2
------
```

## Emphasis

Add emphasis to your text.

```
Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```

Emphasis, aka italics, with _asterisks_ or _underscores_.

Strong emphasis, aka bold, with **asterisks** or **underscores**.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

## Lists

Inline HTML should be used to display lists.

**Ordered List**

```
<ol>
<li>First ordered list item</li>
<li>Another item</li>
</ol>
```

<ol>
<li>First ordered list item</li>
<li>Another item</li>
</ol>

**Unordered List**

```
<ul>
<li>Unordered item 1</li>
<li>item 2</li>
</ul>
```

<ul>
<li>Unordered item 1</li>
<li>item 2</li>
</ul>

**Description List**

```
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>
```

<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>

## Links

There are two ways to create links.

```
[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link to a phone number](tel:1112223333)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../blob/master/LICENSE)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links.
http://www.example.com or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[link text itself]: http://www.reddit.com
```

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link to a phone number](tel:1112223333)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../blob/master/LICENSE)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links.
http://www.example.com or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[link text itself]: http://www.reddit.com

## Images

```
Here's our logo (hover to see the title text):

Inline-style:
![alt text](https://docs.botcopy.com/_assets/whitehorizontal.svg "Logo Title Text 1")

Reference-style:
![alt text][logo]

[logo]: https://docs.botcopy.com/_assets/whitehorizontal.svg "Logo Title Text 2"
```

Here's our logo (hover to see the title text):

Inline-style:

![alt text](https://docs.botcopy.com/_assets/whitehorizontal.svg "Logo Title Text 1")

Reference-style:

![alt text][logo]

[logo]: https://docs.botcopy.com/_assets/whitehorizontal.svg "Logo Title Text 2"

## Blockquotes

```
> Blockquotes can be used to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.
```

> Blockquotes can be used to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can _put_ **Markdown** into a blockquote.

## Code and Syntax Highlighting

````
Inline `code` has `back-ticks around` it.

Inline code has back-ticks around it.

Blocks of code are fenced by lines with three back-ticks ```.
````

Inline `code` has `back-ticks around` it.

Inline code has back-ticks around it.

Blocks of code are fenced by lines with three back-ticks ```.

## Inline HTML

You can also use raw HTML in your Markdown, and it'll mostly work pretty well.

```
<div>
  <div>The div with no name</div>

  <div>Markdown in HTML</div>
  <div>Does *not* work **very** well. Use HTML <em>tags</em>.</div>
</div>
```

<div>
  <div>The div with no name</div>

  <div>Markdown in HTML</div>
  <div>Does *not* work **very** well. Use HTML <em>tags</em>.</div>
</div>

## Line Breaks

Our recommendation for learning how line breaks work is to experiment and discover -- hit `<Enter>` once (i.e., insert one newline), then hit it twice (i.e., insert two newlines), see what happens. Or use `<br>`. You'll soon learn to get what you want.

Here are some things to try out:

```
Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*.
```

Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a _separate paragraph_.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the _same paragraph_.
