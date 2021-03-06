# Pandoc filters

A collection of pandoc filters that I have made. They are written in Lua, [which makes
them faster than filters written in any other language][1]. Copy from the `dist` folder
into `$HOME/.local/share/pandoc/filters`. Use them with `--lua-filter`.

[1]: https://pandoc.org/lua-filters.html

## Usage

### code-includes

Include code from another file by using a code block attribute and the name of the file
inside the code block. This is useful for a couple of reasons:

- You can keep code examples in separate directory and keep everything in sync.
- Large scripts that you don't want to scroll past can be kept in a separate file.

```
~~~{.python inc=yes}
/path/to/file.py
~~~
```

**TODO:** include specific lines

### transclude

Include content from another file just like AsciiDoc and ReST. Is recursive and
works with spaces in file names. **Note the blank lines:**

    text text.

    [/path/to/file.md]{inc=yes}

    text text text.

Note that this requires the `bracketed_spans` extension to be turned on (it
should be on by default). This doesn't resolve internal references, like
footnotes between the files.

If you _do_ want that to work, try [m4(1)](https://www.gnu.org/software/m4/m4.html), a
generic preprocessor that should already be installed on your system. Make sure you
change the default quotation marks with `changequote`.

### standard-code (HTML output only)

Pandoc has great syntax highlighting by default but sometimes you just want to use
something else. Unfortunately, the default pandoc codeblock output with `--no-highlight`
looks like this:

    <pre class="lua"><code>print 'Hello World!'</code></pre>

The above is not what the [W3C recommends][2], nor is it compatible with
[prism.js][3]. This filter makes sure pandoc outputs the recommended syntax:

    <pre><code class="language-lua">print 'Hello World!'</code></pre>

It passes through all classes that don't match a programming language name.

[2]: https://www.w3.org/TR/html5/text-level-semantics.html#the-code-element
[3]: https://prismjs.com

**TODO:** optimize.

### oldschool (still under construction)

Tries to write files in a basic plain format that you might use for a README that is not
meant to be ever rendered. Extends pandoc's `plain` output to add some more goodies like
header underlines and link targets. Use with like this:

    pandoc -s -t plain --lua-filter oldschool.lua -o <output> <input>

Insipired by the READMEs of Bash and Opus.

**TODO:** finish.

## License

Files in this repository are made available under the MIT/Expat License.
